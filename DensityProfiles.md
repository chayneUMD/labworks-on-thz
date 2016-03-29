# Introduction #

Modify density profiles to fit the one used in experiment, using **MATLAB**.


# Original One #
```
  [X,Y]=meshgrid(0:1:200,-50:.5:50);
  Z=(Y>=-30 & Y<=30).*(1+0.45*sin(2*pi/50.*X)+0.5.*Y.^2/(15^2))...
      + (Y>30 & Y<45).*((1+2+0.45*sin(2*pi/50.*X)).*(45-Y)/15)...
      + (Y>-45 & Y<-30).*((1+2+0.45*sin(2*pi/50.*X)).*(45+Y)/15)...
      + (Y>=45).*0 ...
      + (Y<=-45).*0;
  contourf(X,Y,Z);
```

# Modification 1 #
### Adding Factors while keeping rc and ro ###
```
  [X,Y]=meshgrid(0:1:200,-50:.5:50);
  Z=(Y>=-30 & Y<=30).*(3.4+2.65*(sin(2*pi/50.*X))+(1.55+0.5*sin(2*pi/50.*X)).*(0.72-sin(2*pi/50.*X)).*0.5.*Y.^2/(15^2))...
      + (Y>30 & Y<45).*((3.4+2*(1.55+0.5*sin(2*pi/50.*X)).*(0.72-sin(2*pi/50.*X))+2.65*sin(2*pi/50.*X)).*(45-Y)/15)...
      + (Y>-45 & Y<-30).*((3.4+2*(1.55+0.5*sin(2*pi/50.*X)).*(0.72-sin(2*pi/50.*X))+2.65*sin(2*pi/50.*X)).*(45+Y)/15)...
      + (Y>=45).*0 ...
      + (Y<=-45).*0;
  contourf(X,Y,Z);
```

# Modification 2 #
### Generally！  here set rc & ro changes with z axis while keeping n1 constant ###
```
    [X,Y]=meshgrid(0:1:200,-50:.5:50);
  rc=22.5+7.5*sin(2*pi/50.*X-0.5*pi);
  ro=rc+15;
  n0=1+0.7*cos(2*pi/50.*X);
  n1=1.3*X.^0;  %n1 const
  %n1=1.3+0.09*cos(2*pi/50.*X);  %n1 changes little along z
  
  Z=(Y>=-rc & Y<=rc).*(n0+(n1-n0).*(Y.^2./(rc.^2)))...
      + (Y>rc & Y<ro).*(n1.*(ro-Y)./(ro-rc))...
      + (Y>-ro & Y<-rc).*(n1.*(ro+Y)./(ro-rc))...
      + (Y>=45).*0 ...
      + (Y<=-45).*0;
  contourf(X,Y,Z);
```

# Modification 3 #
### Add _Ox horn_ at the area where maximum along Z axis ###
```
  [X,Y]=meshgrid(0:1:200,-50:.5:50);
  rc=22.5+7.5*sin(2*pi/50.*X-0.5*pi);
  ro=rc+15;
  n0=1+0.7*cos(2*pi/50.*X);
  %n1=1.3*X.^0;  %n1 const
  n1=1.3+0.1*cos(2*pi/50.*X);  %n1 changes little along z, delta(n1) positive
  
  Z=(Y>=-rc & Y<=rc).*(n0+(n1-n0).*(Y.^2./(rc.^2)))...
      + (Y>rc & Y<ro).*(n1.*(ro-Y)./(ro-rc))...
      + (Y>-ro & Y<-rc).*(n1.*(ro+Y)./(ro-rc))...
      + (Y>=ro).*0 ...
      + (Y<=-ro).*0;
  contourf(X,Y,Z);
```

# Modification 4 #
### Set delta(n1) _negative_  _No Ox horn_ at the area where maximum along Z axis ###
```
  [X,Y]=meshgrid(0:1:200,-50:.5:50);
  rc=22.5+7.5*sin(2*pi/50.*X-0.5*pi);
  ro=rc+15;
  n0=1+0.7*cos(2*pi/50.*X);
  %n1=1.3*X.^0;  %n1 const
  n1=1.15-0.04*cos(2*pi/50.*X);  %n1 changes little along z
  
  Z=(Y>=-rc & Y<=rc).*(n0+(n1-n0).*(Y.^2./(rc.^2)))...
      + (Y>rc & Y<ro).*(n1.*(ro-Y)./(ro-rc))...
      + (Y>-ro & Y<-rc).*(n1.*(ro+Y)./(ro-rc))...
      + (Y>=ro).*0 ...
      + (Y<=-ro).*0;
  contourf(X,Y,Z);
```
  * Text in **bold** or _italic_
  * Headings, paragraphs, and lists
  * Automatic links to other wiki pages