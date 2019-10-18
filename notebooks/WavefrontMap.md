
# Aufgaben

- Impotiere Daten des Wellenfrontfehlers für das 0 und 10 Grad Feld
- Image Demo Beispiel anschauen
- Bilder des Wellenfrontfehlers darstellen (Achsenbeschriftung nicht vergessen) und Speichern
- Reihenentwicklung als Funktion definieren, Querschnitte darstellen und mit Daten vergleichen



## Inhalt

### 1. [Einleitung](#Einleitung)

### 2. [Öffnen der .txt Dateien](#open_files)

### 3. [Darstellung des Wellenfrontfehlers](#image)


## 1. Einleitung <a name="Einleitung"></a>

Im Folgenden sollen die aufgenommenen Wellenfrontfehler, die im .txt Format gespeichtert wurden, in dieses jupyter-notebook geladen und anschließend als Abbildung gespeichert werden. Um mit den Daten zu arbeiten, wird die Progammiersprache [python](https://www.python.org/) verwendet. Für die Darstellung benutzen wir die Bibliothek Matplotlib. Numpy ist eine Bibliothek, die Operationen mit multi-dimensionalen arrays, sowie Matrizen erlaubt.


- wichtige Bibliotheken für python: 
    - [Numpy](https://www.numpy.org/)
    - [Scipy](https://www.scipy.org/)
    - [Matplotlib](https://matplotlib.org/)

## 2. Öffnen der .txt Dateien <a name="open_files"></a>

- txt Datein des Wellenfrontfehlers sollten im ascii Format gespeichert sein
- zum Laden der Daten in ein numpy array benutzten wir die ```loadtxt``` Funktion aus der numpy Bibliothek
- die numpy Bibliothek wird über `import numpy as np` importiert
- für Kommandos auf dem Betriebssystem verwenden wir das [os-module](https://docs.python.org/2/library/os.html)


```python
import numpy as np # import statement
import os # os module
cwd = os.getcwd() ## get the current working directory
zero_deg_array = np.loadtxt(os.path.join(cwd,
                                         'textfiles', 
                                         'wavefront_0_deg_ascii.txt')) # loadtxt funtion, with path to the txt file 

```


```python
zero_deg_array # wavefront error for the zero degree field
```

## 3.  Darstellung des Wellenfrontfehlers <a name="image"></a>

Der Werte des Wellenfrontfehlers sind nun unter der Variablen `zero_deg_array` aufrufbar. Mithilfe des unten angebenen Beispiels, kann nun ein Bild erzeugt werden. Ersetze dazu die Variable `Z` mit `zero_deg_array` in `ax.imshow`. 


```python
figure_folder = "pictures" # folder path for picture
figure_filename = "example.png" # !change figure file name for your wavefront map!
```

### Example: [Image Demo](https://matplotlib.org/gallery/images_contours_and_fields/image_demo.html)

- Note: Use `%matplotlib inline first



```python
%matplotlib inline
import numpy as np
import matplotlib.cm as cm
import matplotlib.pyplot as plt

delta = 0.025
x = y = np.linspace(1.0, -1.0, zero_deg_array.shape[0]) # x and y coordinates
X, Y = np.meshgrid(x, y) # 
Z1 = np.exp(-X**2 - Y**2) # Gaussian dummy function 1
Z2 = np.exp(-(X - 1)**2 - (Y - 1)**2) # Gaussian dummy function 2
Z = (Z1 - Z2) * 2 # final function to show 

fig, ax = plt.subplots()
im = ax.imshow(Z, interpolation='bilinear', cmap=cm.RdYlGn,
               origin='lower', extent=[-1, 1, -1, 1])
plt.colorbar(im)
# look in matplotlib examples to add x-and y-labels!
plt.savefig(os.path.join(cwd, figure_folder, figure_filename), 
            format='png') # savefig saves the image in the pictures folder

plt.show()
print("Figure saved in {0:20s}".format(os.path.join(cwd, figure_folder, figure_filename)))
```

## 4. Reihenentwicklung <a name="wavefrontexpansion"></a>


```python
def wavefront_on_axis(w000, w020, w040, pupil_sampling=128):
    """
    wavefront expansion for the on axis field
    
    Parameters
    ----------
    w000 : float
        Piston
    w020 : float
        Focus
    w040 : float
        spherical Aberration
    pupil_sampling : int
        grid size of the sampled pupil 
    
    Returns
    -------
    wavefront error for the on axis field, up to 4th order
    """
    x = np.linspace(1, -1, pupil_sampling)
    y = np.linspace(1, -1, pupil_sampling)
    X, Y = np.meshgrid(x, y)
    
    return w000 + w020 * (X**2 + Y**2) + w040 * (X**2 + Y**2)**2
    
    
```


```python
# w_0 = wavefront_on_axis()
```


```python
%matplotlib inline
#print("cross section at y={0:2.2f}".format(y[ w_0.shape[0]//2]))
#plt.figure()
#plt.plot(x, w_0[:, w_0.shape[0]//2])
#plt.plot(x, zero_deg_array[:, w_0.shape[0]//2], '--')
#plt.xlabel('$x_p$')
#plt.ylabel('$W_0$ in $\lambda$')
#plt.show()
# cross section at position x, y=..
#sclice = 30
#print("cross section at y={0:2.2f}".format(y[ w_0.shape[0]//2 + sclice]))
#plt.figure()
#plt.plot(x, w_0[:, w_0.shape[0]//2 + sclice])
#plt.plot(x, zero_deg_array[:, w_0.shape[0]//2 + sclice], '--')
#plt.xlabel('$x_p$')
#plt.ylabel('$W_0$ in $\lambda$')
#plt.show()
```


```python
def wavefront_pupil_edge(w000, w111, w020, w040, w131, w222, w311, w220, pupil_sampling=128):
    """
    wavefront expansion at the edge of the pupil
    
    Parameters
    ----------
    w000 : float
        Piston
    w111 : float
        Tilt
    w020 : float
        Focus
    w040 : float
        spherical Aberration
    w131 : float
        Coma
    w222 : float
        Astigmatismus
    w311 : float
        Distortion
    w220 : float
        Field Curvature
    pupil_sampling :int
    
    
    Returns
    -------
    wavefront error for the pupil edge, up to 4th order
    
    """
    x = np.linspace(1, -1, pupil_sampling)
    y = np.linspace(1, -1, pupil_sampling)
    X, Y = np.meshgrid(x, y)
    
    return 
    
```


```python
# w_10 = wavefront_pupil_edge()
```


```python
ten_deg_array = np.loadtxt(os.path.join(cwd, 'textfiles', 
                                        'wavefront_10_deg_ascii.txt')) # loadtxt funtion, with path to the txt file 
```
