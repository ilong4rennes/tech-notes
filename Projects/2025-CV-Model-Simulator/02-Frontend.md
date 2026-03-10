---
created: 2025-07-06
tags:
  - frontend
  - "#nextjs"
org:
  - GC
goal: Implement the frontend using Next.js
---
Reference: 
- [[20250702-Design Doc|Design Doc]]

## Sidebar
### Choose Model from List

Functionalities:
- User clicks on the model name to select this model
- Remember the model id so that when clicks the run button afterwards, the model id can be passed to backend. 

Problem: Right now in the sidebar, the model id and their names are hardcoded. When user clicks on the model, it only displays the frontend changes, without actually updating the 'model chosen' information. We do not want that. How to 'remember' this information?
Solution: Use [[React Global State]]. Set up Redux to remember the selected model id. 
### Add Model Button

Functionalities:
- When clicked, user can select and upload a `.pt` file from file system.
- Then a pop up window will be displayed for the users to enter the name of the model. 
- If success, a new model with the input model name will be shown in the sidebar. 
- It will send HTTP request to backend in form-data format, including the `.pt` file and its name. 
- It triggers the **POST /api/models** api in the backend.

Component: `AddModelButton.tsx`

#### Error
```bash
✓ Compiled /_not-found/page in 255ms
 ⨯ [Error: Body exceeded 1 MB limit.
                To configure the body size limit for Server Actions, see: https://nextjs.org/docs/app/api-reference/next-config-js/serverActions#bodysizelimit] {
  statusCode: 413
}
```
- This is an front-end error related to [[Next.js Server Actions]] which occurs because the `.pt` file we uploaded is too large, exceeding Next.js's default body size of 1MB. 

### Remove Model Button

Functionalities:
- For all the user-uploaded models displayed in the sidebar, there should be a delete button on its side.
- When clicked, the model will be deleted and not be shown in the sidebar.
- It triggers the **DELETE /api/models/:id** api in the backend.

## Input
### Run Inference Button

Functionalities:
- When clicked, it should send an HTTP request to backend in form-data format, including:
	- model id
	- temp path to file (image/video)
- If model id is not specified, there should be a pop up window telling user to select a model. 

Question:
- How does the backend know which model file corresponds to this model id? Are we going to store the pairs somewhere?

Answer:
- After the frontend sends the request, the backend will look up `model.json` to find the file path which belongs to the model id, load the `.pt` file , and run the model on the uploaded image. 

我现在需要：
1. update tickets
	1. 对比什么做完了 什么没做完
	2. 没做完的东西拉一个表
	3. 

我需要完成如下：

1. 

目前我收集到的信息有：

- 

请从头到尾帮我理清思路，step by step
