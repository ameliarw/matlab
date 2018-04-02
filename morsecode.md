#### Matlab Code
```Matlab
function rules(n)
%remember, n refers to the number of eigenvectors generated
%here are my constants: 
%m is mass, L is length of box, barht height of barrier, w is the barrier
%width, k = spring constant, q = charge

m = 1;
L = 3;
hbarsq = 1; 
barht = 1e6;
w=3; 
pts = 250;
k = 1e3;
q = 1;
n=5;

%now account for the delta x and discritize the number of elements in the x vector 
%We say that there are some number of points within the x vector
x = linspace(-1, L, pts);
dx = x(2)-x(1);

%x is a vector that goes from 0 to 5 separated by some amount, dictated by
%the number of points. Vvec is a vector with the dimension of pts entries
%with one column
%now we have some number, points of entries in the x vector

% morse potential 
Vvec = 1*(1-exp((-1*(x-1).^2)));
Vvec = Vvec.*100;
% now scaled so we can see the states within the well

V = diag(Vvec);

%making the second derivative matrix
D2 = -2*eye(pts) + diag(ones(pts-1,1), 1) + diag(ones(pts-1,1),-1);

%now account for the delta x idea by subtracting the first element from the
%second element because they will be evenly spaced, and multiply the matrix
%by the constants 
T = 1/(dx^2)*(-hbarsq/(2*m))*D2;
H=T+V; % Hamilotian operator matrix (H)

%now we want to solve for the eigenvalues of the square matrix H
[vecs, vals] = eig(H);

%now we sort the vectors and values so that they are plotted in ascending
%order of n, but the eigenvalues and eigenvectors stay together
[srtvecs,srtvals]=eigsort(vecs,vals); 

%scaling the srtvecs values
sc=100;
srtvecs=sc*srtvecs;
v=diag(srtvals);% vector of sorted eigenvalues

%now we will need to shift the graphs up so that they are on different
%levels, as determined by energy
repvals=(ones(pts,1))*v';

%defining shiftvecs so that the graphs will be shifted up
shiftvecs=srtvecs+repvals; 

srtvecs = real(srtvecs);

%is the transition allowed? 
transitions = srtvecs' * diag(x) * srtvecs;

figure (2); 
pcolor(abs(transitions(1:9, 1:9).^2));
axis square

%adjust axis 
figure(1);plot(x,shiftvecs(:,1:n),x,Vvec); 
axis([-inf inf -.5 200]); 
end
```