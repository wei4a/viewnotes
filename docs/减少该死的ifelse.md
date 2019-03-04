```java
//校验必填项
        						if(null != flowName && !"".equals(flowName)){
        							//获取流程flowid
        							String flowIdSql = "select flowId from taw_system_workflow flow where flow.remark='"+flowName+"'";
        							List flowIdList = is.getSheetAccessoriesList(flowIdSql);
        							if(flowIdList != null && !flowIdList.isEmpty()){
        								Map map = (Map) flowIdList.get(0);
        								flowId = StaticMethod.nullObject2String(map.get("flowId"));
        								
        								//校验角色
        								if(null != roleName && !"".equals(roleName)){
        									//校验角色是否存在
        									String roleSql = "select role_id from taw_system_role where role_name='"+roleName+"' and workflow_flag='"+flowId+"' ";
        									List roleList = is.getSheetAccessoriesList(roleSql);
        									if(roleList != null && !roleList.isEmpty()){
        										Map roleMap = (Map) roleList.get(0);
        										roleId = StaticMethod.nullObject2String(map.get("role_id"));
        										
        										//校验子角色
        										if(null != subRoleName && !"".equals(subRoleName)){
        											String subRoleSql = "select id from taw_system_sub_role a where a.roleid='"+roleId+"' and a.subrolename='"+subRoleName+"'";
        											List subRoleList = is.getSheetAccessoriesList(subRoleSql);
        											if(subRoleList != null && !subRoleList.isEmpty()){
        												Map subRoleMap = (Map) subRoleList.get(0);
        												String subRoleId = StaticMethod.nullObject2String(subRoleMap.get("id"));
        												
        												//校验操作类型是否存在
    													if(null != opreateType && !"".equals(opreateType)){
    														if("新增".equals(opreateType)||"解除关联".equals(opreateType)||"保持不变".equals(opreateType)){
    															
    															//校验用户是否存在
    	        												String userSql = "select * from taw_system_userrefrole where subroleid='"+subRoleId+"' and userId =''"+userId+"'";
    	        												List userList = is.getSheetAccessoriesList(userSql);
    	        												if(userList != null && !userList.isEmpty()){
    	        													
    	        													Map userMap = (Map) userList.get(0);
    	        													String id = StaticMethod.nullObject2String(userMap.get("id"));
    	        													if("解除关联".equals(opreateType)){
    	        														String delUserSql = " delete from taw_system_userrefrole where  id ='"+id+"'";
    	        													}else if("新增".equals(opreateType)){
    	        														errorMessage = "用户名"+userId+"在"+subRoleName+"中已经存在！";
    	        														resultMap.put("errorMessage", errorMessage);
    	        														resultMap.put("flag", "false");
    	        													}
    	        												}else{
    	        													if("新增".equals(opreateType)){
            															//添加用户
        																String addUserSql = "insert into taw_system_userrefrole ("
        																		+ " id,USERID,SUBROLEID,GROUPTYPE,ROLETYPE)"
        																		+ " values('"+UUIDHexGenerator.getInstance().getID()+"',"
        																				+ "'"+userId+"',"
        																				+ "'"+subRoleId+"',"
        																				+ "'1',"
        																				+ "'1')";
        																is.updateTasks(addUserSql);
        																
            														}else if("解除关联".equals(opreateType)){
            															//删除用户
            															errorMessage = "用户名"+userId+"在"+subRoleName+"中不存在！";
    	        														resultMap.put("errorMessage", errorMessage);
    	        														resultMap.put("flag", "false");
            															
            														}else{
            															//保持不变
            															errorMessage = "用户名"+userId+"在"+subRoleName+"中不存在！";
    	        														resultMap.put("errorMessage", errorMessage);
    	        														resultMap.put("flag", "false");
            														}
    	        												}
    															
    															
    														}else{
    															errorMessage = "操作类型填写错误！";
        					        			                resultMap.put("errorMessage", errorMessage);
        					        			                resultMap.put("flag", "false");
    														}
    														
    														
    													}else{
    														errorMessage = "操作类型为必填！";
    					        			                resultMap.put("errorMessage", errorMessage);
    					        			                resultMap.put("flag", "false");
    													}
        												
        												
        											}else{
        												errorMessage = "子角色"+subRoleName+"在"+roleName+"中不存在！";
                            			                resultMap.put("errorMessage", errorMessage);
                            			                resultMap.put("flag", "false");
        											}
        										}else{
        											errorMessage = "子角色名称为必填！";
                        			                resultMap.put("errorMessage", errorMessage);
                        			                resultMap.put("flag", "false");
        										}
        										
        									}else{
        										errorMessage = "角色"+roleName+"在"+flowName+"中不存在！";
                    			                resultMap.put("errorMessage", errorMessage);
                    			                resultMap.put("flag", "false");
        									}
        								}else{
        									errorMessage = "角色名称为必填！";
                			                resultMap.put("errorMessage", errorMessage);
                			                resultMap.put("flag", "false");
        								}
        								
        							}else{
        								errorMessage = "流程名称填写错误！";
            			                resultMap.put("errorMessage", errorMessage);
            			                resultMap.put("flag", "false");
        							}
        						}else{
        							errorMessage = "流程名称为必填！";
        			                resultMap.put("errorMessage", errorMessage);
        			                resultMap.put("flag", "false");
        						}
        					}
        				}
        			}
        		}
        		
        	}else{
        		errorMessage = "模板格式有误，请上传正确的模板！";
                resultMap.put("errorMessage", errorMessage);
                resultMap.put("flag", "false");
        	}
        }else{
        	 errorMessage = "模板格式有误，请上传正确的模板！";
             resultMap.put("errorMessage", errorMessage);
             resultMap.put("flag", "false");
        }
```

