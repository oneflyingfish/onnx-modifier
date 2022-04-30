<img src="./docs/onnx_modifier_logo_1.png" style="zoom: 60%;" />

# Introduction

To edit an ONNX model, One common way is to visualize the model graph, and edit it using ONNX Python API. This works fine. However, we have to code to edit, then visualize to check. The two processes may iterate for many times, which is time-consuming. :wave: 

What if we have a tool, which allow us **edit and preview the editing effect in a totally visualization fashion**? 

Then `onnx-modifier` comes. With it, we can focus on editing the model graph in the visualization pannel. All the editing information will be summarized and  processed by Python ONNX automatically at last. Then our time can be saved! :rocket: 

`onnx-modifier` is built based on the popular network viewer [Netron](https://github.com/lutzroeder/netron) and the lightweight web application framework [flask](https://github.com/pallets/flask). 

Currently, the following editing operations are supported:

- Delete a single node.
- Delete a node and all the nodes rooted with it.
- Recover a deleted node.
- Rename the input/output name.

Hope it helps!

# Get started 

Install the require Python packages by 

```bash
pip install onnx
pip install flask
```

Then run

```bash
python app.py
```

Click the url in the output info generated by flask (`http://127.0.0.1:5000/` for example), then `onnx-modifier` will be launched in the web browser. 

Click `Open Model...` to upload the onnx model to edit. The model will be parsed and show on the page.

# Edit

<table>
    <tr>
        <td ><center><img src="./docs/top_left_buttons.png"> top left buttons (Graph-level-operations)</center></td>
        <td ><center><img src="./docs/node_prop_buttos.png" >sidebar buttons (Node-level-operations)</center></td>
    </tr>
<table>

Graph-level-operation elements are placed on the left-top of the page. Currently, there are three buttons: `Preview`，`Reset` and `Download`. They can do:

- `Preview`：Preview the result model graph with all current modifications applied ；
- `Reset`：Reset the model graph to its initial state；
- `Download`：save the modified model file.

Node-level-operation elements are all in the sidebar, which can be invoked by clicking a specific node. Let's take a closer look.

## Delete node

There are two modes (buttons) for deleting node: `Delete With Children` and `Delete Single Node`. `Delete Single Node` only deletes the clicked node, while `Delete With Children` also deletes all the node rooted on the clicked node, which is convenient and nature if we want to delete a long path of nodes. 

> The implementation of `Delete With Children` is based on backtracking algorithm.

The deleted nodes are in grey mode. The following figure shows a typical deleting process.

<img src="./docs/onnx_modifier_delete.png" style="zoom: 50%;" />

## Recover node

 By `Recover Node` button, we can recover the node back to graph after deleting it.

## Change the input/output name of node

By changing the input/output name of nodes, we can change the model forward routine. It can also be helpful if we want to rename the model output(s). 

How can we do this using `onnx-modifier`? Note that there is a `RENAME HELPER` section in the node sidebar. All the original input and output name of a node is listed here, each following with a input field. we can input the new name here. After clicking  the `Preview` button, the graph will be rendered with the new name (and a new model forward routine). 

For example,  Now we want remove the preprocess operators (`Sub->Mul->Sub->Transpose`) shown in the following figure. We can

1. click on the first `Conv` node, rename the original input name as *serving_default_input:0*.
2. delete the node between input node and the first `Conv` 
3. click `Preview` to preview, then click `Download`, we can get the modified ONNX model.

<img src="./docs/rename_node_io.png" alt="rename_node_io" style="zoom:60%;" />



`onnx-modifier` is under active development :hammer_and_wrench:. Welcome to use, create issues and pull requests! 🥰

# Credits and referred materials

- ONNX Python API [Official](https://github.com/onnx/onnx/blob/main/docs/PythonAPIOverview.md) [Leimao's Blog](https://leimao.github.io/blog/ONNX-Python-API/)
- ONNX IO Stream  [Leimao's Blog](https://leimao.github.io/blog/ONNX-IO-Stream/)
- [Netron](https://github.com/lutzroeder/netron)
- [onnx-utils](https://github.com/saurabh-shandilya/onnx-utils)
- [flask](https://github.com/pallets/flask)
