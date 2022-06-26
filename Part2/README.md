# VTK & Qt

![gif](https://github.com/sbme-tutorials/cg-task3-teamseven/blob/main/Part2/Part2/Part2.gif)

```C++
def ray_casting(self):
                self.volumeMapper = vtk.vtkSmartVolumeMapper()
                self.volumeMapper.SetInputData(self.imageData)
                self.volumeColor = vtk.vtkColorTransferFunction()
                self.volumeColor.AddRGBPoint(0,    0.00, 0.000, 0.00) 
                self.volumeColor.AddRGBPoint(1,    0.00, 0.000, 0.00) 
                self.volumeColor.AddRGBPoint(100,    1.00, 0.000, 0.00)   #mold
                self.volumeColor.AddRGBPoint(500,    1.0, 1.00, 1.00)   #skin1
                self.volumeColor.AddRGBPoint(1000,   1.00, 1.00, 1.00)   #skin2
                self.volumeColor.AddRGBPoint(1500,   1.0, 1.0, 1.0) #bones
                self.volumeColor.AddRGBPoint(2000,   0, 0, 0)    #dense bones e.g. teeth 
                
                self.volumeScalarOpacity = vtk.vtkPiecewiseFunction()
                self.volumeScalarOpacity.AddPoint(0,  0.00)
                self.volumeScalarOpacity.AddPoint(1,  0.00)
                self.volumeScalarOpacity.AddPoint(100,  1.00)
                self.volumeScalarOpacity.AddPoint(500,  1.0000)
                self.volumeScalarOpacity.AddPoint(1000, 1.000)
                self.volumeScalarOpacity.AddPoint(1500, 1.0)
                self.volumeScalarOpacity.AddPoint(2000, 0.00)

```
#### The opacity transfer function is used to control the opacity of different tissue types.



```C++
volumeGradientOpacity = vtk.vtkPiecewiseFunction()

                volumeGradientOpacity.AddPoint(0,   1.0)
                volumeGradientOpacity.AddPoint(1,   1.0)
                volumeGradientOpacity.AddPoint(100,   1.0)
                volumeGradientOpacity.AddPoint(100,   1.0)
                volumeGradientOpacity.AddPoint(500, 1.0)
                volumeGradientOpacity.AddPoint(1000,  1.0)
                volumeGradientOpacity.AddPoint(1500, 1.0)
```
### The gradient opacity function is used to decrease the opacity in the "flat" regions of the volume while maintaining the opacity at the boundaries between tissue types.  The gradient is measured as the amount by which the intensity changes over unit distance.For most medical data, the unit distance is 1mm.

`volumeProperty = vtk.vtkVolumeProperty()` The VolumeProperty attaches the color and opacity functions to the volume, and sets other volume properties.  The interpolation should be set to linear to do a high-quality rendering.  The ShadeOn option turns on directional lighting, which will usually enhance the appearance of the volume and make it look more "3D".  However, the quality of the shading depends on how accurately the gradient of the volume can be calculated, and for noisy data the gradient estimation will be very poor.  The impact of the shading can be decreased by increasing the Ambient coefficient while decreasing the Diffuse and Specular coefficient.  To increase the impact of shading, decrease the Ambient and increase the Diffuse and Specular.

```C++
 self.volume = vtk.vtkVolume()
                self.volume.SetMapper(self.volumeMapper)
                self.volume.SetProperty(volumeProperty)
                self.rc_rendering()
                # Finally, add the volume to the renderer
```
### The vtkVolume is a vtkProp3D (like a vtkActor) and controls the position and orientation of the volume in world coordinates.

```C++
def rc_rendering(self):
    
                self.renWin = qvtk(self.frame)
                self.ren = vtk.vtkRenderer()
                camera =  self.ren.GetActiveCamera()
                c = self.volume.GetCenter()
                camera.SetFocalPoint(c[0], c[1], c[2])
                camera.SetPosition(c[0] + 400, c[1], c[2])
                camera.SetViewUp(0, 0, -1)
```
### Set up an initial view of the volume.  The focal point will be the center of the volume, and the camera position will be 400mm to the patient's left (whis is our right).

`renWin.SetSize(640, 480)` Increase the size of the render window
### Interact with the data.

```C++
                self.iren.Initialize()
                self.renWin.Render()
                self.iren.Start()
                
```


### Problems Faced

### 1)Techinal problems when install vtk using anaconda

```C++
      ERROR: Could not find a version that satisfies the requirement python3-vtk7 (from versions: none)
      ERROR: No matching distribution found for python3-vtk7         
```


### We faced another problem regarding the version of python installed
```C++
UnsatisfiableError: The following specifications were found
to be incompatible with the existing python installation in your environment:

Specifications:

  - vtk -> python[version='>=3.5,<3.6.0a0|>=3.6,<3.7.0a0|>=3.7,<3.8.0a0']

Your python: python=3.8

If python is on the left-most side of the chain, that's the version you've asked for.
When python appears to the right, that indicates that the thing on the left is somehow
not available for the python version you are constrained to. Note that conda will not
change your python version to a different minor version unless you explicitly specify
that.

```


### 2)Problems when connecting sliders with GUI
#### It was hard at the beginning to make the window updated with the sliders.




> Done by : <br>
> mennat allah kamel nawar  ID : 1170354 <br>
> mennat allah hesham ID : 1180155 <br>
> Fatma essam mohammed ID : 1180606 <br>
> Farah ossama ID : 1180456 <br>
> Pierre amir ID : 1180214
