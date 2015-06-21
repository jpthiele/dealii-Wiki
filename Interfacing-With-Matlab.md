# A few hints about interfacing with Matlab

## Introduction

While deal.II offers a lot of possibilities to output data in different formats to produce awesome graphics, sometimes you experience the need to evaluate the data directly. Most of the deal.II classes have methods to temporarily write out raw data (e.g. Vector::block_write, etc.), and maybe you find it useful to load and modify the data in an environment like Matlab. The following scripts enable reading and writing of deal.II vector data, codes to process matrices may follow.

These codes should be easy to port to Octave.

<wiki:toc  />

## Reading vectors

```matlab
function v = read_deal_vec(file, accuracy);
% function vect = read_deal_vec(file);
% function vect = read_deal_vec(file, accuracy);
%
% reads in a DEAL.II vector, written by Vector<>::block_write().
% The vector is stored in "file", which can be either a filename or
% a file handle. If you supply a filename, the file will be closed
% automatically.
% You usually only supply a file handle if more than one vector is
% stored within the file.
% "Accuracy" can be given if you stored a vector storing floats or
% long doubles. It is by default set to 'double'.
%
% Ralf B. Schulz, 2003--2005

if(nargin < 2),  accuracy = 'double'; end;

if(isnumeric(file))
    closefile = 0;
else
    file = fopen(file, 'r');
    closefile = 1;
end;

n = fscanf(file,'%d',1)
v = zeros(1,n);
while(fscanf(file,'%c',1) ~= '[end;

v = fread(file, n, accuracy);

if(fscanf(file,'%c',1) ~= ']('),)'), error('wrong file format!'); end;

if(closefile)
    fclose(file);
end;
```

## Writing Vectors

```matlab
function write_deal_vec(file, vec, accuracy);
% function write_deal_vec(file, vector);
% function write_deal_vec(file, vector, accuracy);
%
% writes out a DEAL.II vector that can be read in using
% Vector<>::block_read(). The vector is stored in "file", 
% which can be either a filename or a file handle. If you 
% supply a filename, the file will be closed automatically.
% You usually supply a file handle if more than one vector is
% to be written to the file.
% "Accuracy" can be given if you stored a vector storing floats or
% long doubles. It is by default set to 'double'.
%
% Ralf B. Schulz, 2003--2005

if(nargin<3)
    accuracy = 'double';
end;

vec = vec(:);

if(isnumeric(filename))
   file = filename;
   closefile = 0;
else
    file = fopen(filename, 'w');
    closefile = 1;
end;

fprintf(file,'%d\n[length(vec));
fwrite(file, vec, accuracy);
fprintf(file,'](',)\n');

if(closefile)
    fclose(file);
end;
```
