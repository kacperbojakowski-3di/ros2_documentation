.. redirect-from::

    Tutorials/Intermediate/RViz/Marker-Sending-Basic-Shapes/Marker-Sending-Basic-Shapes

.. meta::
   :contentType: tutorial
   :experience: beginner
   :area: visualization, tools
   :distribution: {DISTRO}
   :product: {PRODUCT}

.. _LearningDataVisualization:

Learning about data visualization - tutorial
============================================

RViz is a 3D visualizer for ROS.
This article walks you through starting RViz, showing simple shapes from a marker publisher, and controlling the 3D view.
A hands-on exercise with sample marker shapes gives you practice viewing 3D data in RViz.

**Area: Visualization, Tools | Content-type: tutorial | Experience: beginner**

.. contents:: Contents
   :depth: 3
   :local:

Summary
-------

RViz draws 3D data from your ROS system, such as robot models, sensor readings, and primitive shapes.
A display is an RViz plugin that draws one kind of data in the 3D view.

The ``Marker`` display can show shapes that a node publishes as ``visualization_msgs/msg/Marker`` messages.
RViz does not need to know the meaning of that data ahead of time.

This tutorial uses a small publisher that cycles through a cube, sphere, arrow, and cylinder.
Use that publisher as sample data for setup, viewing, and camera control in RViz.

For a broader RViz reference, see :doc:`RViz User Guide <../RViz-User-Guide/RViz-User-Guide>`.

Prerequisites
-------------

#. :doc:`Install ROS <../../../../Get-Started/Installation>` and :doc:`set up your environment <../../../../Get-Started/Configuring-ROS2-Environment>`.
#. :doc:`Create a workspace <../../../../ROS-Framework/client-libraries/Working-with-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace>`.

Steps
-----

.. note::
   Source ROS in every new terminal you open.
   See :doc:`Configuring environment <../../../../Get-Started/Configuring-ROS2-Environment>`.
   After you build the tutorial package, also source your workspace overlay.

1 Get the marker publisher
^^^^^^^^^^^^^^^^^^^^^^^^^^

This tutorial uses the ``visualization_marker_tutorials`` package from the `visualization_tutorials repository <https://github.com/ros-visualization/visualization_tutorials>`__.
The ``basic_shapes`` executable publishes one marker at a time on ``/visualization_marker``.

Clone the repository into the ``src`` directory of your workspace, then build the ``visualization_marker_tutorials`` package.
The ``-b`` option selects the branch for this ROS distribution:

.. code-block:: console

   $ cd ~/ros2_ws/src
   $ git clone -b {REPOS_FILE_BRANCH} https://github.com/ros-visualization/visualization_tutorials.git
   $ cd ~/ros2_ws
   $ colcon build --packages-select visualization_marker_tutorials

If your workspace is not ``~/ros2_ws``, use your workspace path instead.

2 Start the marker publisher
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Open a new terminal, source your workspace overlay, and run ``basic_shapes``:

.. code-block:: console

   $ cd ~/ros2_ws
   $ source install/setup.bash
   $ ros2 run visualization_marker_tutorials basic_shapes

Leave this terminal running.
The node publishes a new shape about once a second, replacing the previous marker.

The source for that node is `basic_shapes.cpp <https://github.com/ros-visualization/visualization_tutorials/blob/{REPOS_FILE_BRANCH}/visualization_marker_tutorials/src/basic_shapes.cpp>`__.
You do not need to edit it for this tutorial.

3 Start RViz
^^^^^^^^^^^^

Open a new terminal and start RViz:

.. code-block:: console

   $ ros2 run rviz2 rviz2

You do not need the workspace overlay in this terminal.
``rviz2`` comes from your ROS installation.

When RViz starts for the first time, you should see a window like the following:

.. image:: ../RViz-User-Guide/images/initial_startup.png

The large black area is the 3D view.
It is empty until a display has something to draw.

On the left is the Displays list.
It already contains **Global Options** and a **Grid**.
**Fixed Frame** defaults to ``map``.
**Global Status** may show a warning because the sample publisher does not publish transforms.
That is expected.

On the right is the Views panel.
If it is hidden, open it from the **Panels** menu.

4 Set the fixed frame
^^^^^^^^^^^^^^^^^^^^^

RViz uses the fixed frame as the world reference for the 3D view.

The marker publisher uses the frame ``my_frame``.
It does not publish transforms, so RViz must use that same frame.

In the Displays list, expand **Global Options** if needed, then set **Fixed Frame** to ``my_frame``.
Type the name without a leading slash.

If **Fixed Frame** stays at ``map``, the marker will not show in the 3D view.

5 Add a Marker display
^^^^^^^^^^^^^^^^^^^^^^

A Marker display subscribes to marker messages and draws them in the 3D view.

At the bottom of the Displays list, select **Add**:

.. image:: ../RViz-User-Guide/images/add-button.png

The **Create visualization** dialog opens on the **By display type** tab:

.. image:: ../RViz-User-Guide/images/add-display-dialog.png

In the list, find ``rviz_default_plugins``.
**OK** stays unavailable until you select a display type.
**Marker** is further down that group, after **TF** and **TwistStamped**.
Scroll to **Marker**, select it, then select **OK**.

The default topic is ``/visualization_marker``.
That is the topic ``basic_shapes`` publishes.

You should now see a green marker at the origin that changes shape about once a second: cube, sphere, arrow, then cylinder.

.. image:: images/basic_shapes_tutorial.png

The Marker display status should be **OK**.
**Global Status** can stay at **Warn** because nothing is publishing transforms.
If the Marker status is a warning or error, expand the Marker entry and check the topic name and the fixed frame.

6 Change the view
^^^^^^^^^^^^^^^^^

Use the Views panel to choose how RViz controls the camera and how it projects the 3D scene.

.. image:: ../RViz-User-Guide/images/camera-types.png

Camera types combine a control scheme with a projection (perspective or orthographic).

The default type is **Orbit**.
The camera rotates around a focal point and always looks at that point.

To try another camera type, open the **Type** list in the Views panel:

* **Orbit**: Rotate around a focal point.
  This is the default and the one used in the next step.
* **FPS**: Look around from the camera position.
* **TopDownOrtho**: Look down the fixed-frame Z axis.
  Objects do not get smaller with distance.

The **Save** button on the Views panel stores a named view, including the camera type, pose, and target frame.

.. image:: ../RViz-User-Guide/images/views.png

7 Move the camera
^^^^^^^^^^^^^^^^^

Select the **Move Camera** tool on the toolbar.
On first start, **Interact** may already be selected.

The keyboard shortcut is ``m``.

When **Move Camera** is selected, clicking in the 3D view moves the camera using the current view type.

Stay on **Orbit** for this step.
The focal point is shown as a small disc while you move the camera:

.. image:: ../RViz-User-Guide/images/focal-point.png

Try the following:

* **Left mouse button**: Click and drag to rotate around the focal point.
* **Middle mouse button**, or **Shift** and **left mouse button**: Click and drag to move the focal point.
* **Right mouse button**: Click and drag to zoom.
  Drag up to zoom in, and down to zoom out.
* **Scroll wheel**: Zoom in and out of the focal point.

The status bar also summarizes these controls when **Move Camera** is selected: left-click rotates, middle-click moves, right-click or the scroll wheel zooms.

Rotate, pan, and zoom until you can see the changing marker clearly against the grid.

8 Clean up
^^^^^^^^^^

When you are done, enter ``Ctrl+C`` in the ``basic_shapes`` terminal and in the RViz terminal.

Related content
---------------

More articles:

* :doc:`RViz User Guide <../RViz-User-Guide/RViz-User-Guide>`
* :doc:`Marker: Display types <../Marker-Display-types/Marker-Display-types>`
* :doc:`Marker: Points and Lines <../Marker-Points-and-Lines/Marker-Points-and-Lines>`
* :doc:`Learning about topics <../../../../ROS-Framework/interfaces/topics/Understanding-ROS2-Topics/Understanding-ROS2-Topics>`

FAQs
----

What is RViz?
   RViz is a 3D visualizer for ROS.
   It draws data from your system in a 3D view, using displays such as Grid, Marker, and RobotModel.

Why do I set the fixed frame to ``my_frame``?
   The sample publisher puts its markers in the ``my_frame`` frame.
   It does not publish transforms, so RViz must use that same frame as **Fixed Frame**.

Why is the 3D view black or empty?
   RViz has nothing to draw until you add a display that is receiving data.
   Start ``basic_shapes``, set **Fixed Frame** to ``my_frame``, and add a Marker display on ``/visualization_marker``.
   If **Fixed Frame** is still ``map``, the marker will not appear.

How do I rotate, pan, or zoom the 3D view?
   Select the **Move Camera** tool (shortcut ``m``).
   **Interact** may be selected when RViz starts.
   Keep the **Orbit** view, then use the mouse in the 3D view.
   Left-drag rotates, middle-drag or Shift-left-drag moves the focal point, and right-drag or the scroll wheel zooms.
