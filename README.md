clc;
clear all;
close all;
I=imread('brd1c.bmp');  //The image to be used for steganography
I=imresize(I,[256 256]);
imshow(I);title('Cover Image')

txt=textread('steg messg1.txt','%c','whitespace','   '); // The text used to hide in the image 
disp(['Embedding text is']);
txt'
t1=numel(txt);
bt=dec2bin(txt,8);
s2=size(bt);
imbp1=zeros(length(bt),8);
for i=1:length(bt)
    for j=1:4    
        unb(i,j)=(bt(i,j));
        lnb(i,j)=(bt(i,j+4));       
    end
end

key=dec2bin(567,10);

RP=I(:,:,1);

for ii=1:length(lnb)
    p1=dec2bin(RP(ii),8);
    p1(5:8)=(lnb(ii,:));
    RP(ii)=bin2dec(p1);
end


RG=I(:,:,2);

for ii=1:length(unb)
    p1=dec2bin(RG(ii),8);
    p1(5:8)=(unb(ii,:));
    RG(ii)=bin2dec(p1);
end

BP=I(:,:,3);


for ii=1:length(key)
    p1=dec2bin(BP(ii),8);
    p1(8)=num2str(key(ii));
    BP(ii)=bin2dec(p1);
end

SIMG(:,:,1)=RP;
SIMG(:,:,2)=RG;
SIMG(:,:,3)=BP;
 figure,imshow(SIMG);title('stego image')
%=====================
%==Decryption ======

RPP=SIMG(:,:,1);
GPP=SIMG(:,:,2);
BPP=SIMG(:,:,3);

for ii=1:length(lnb)
    p2=dec2bin(RPP(ii),8);
    exm2(ii,:)=p2(5:8);
    p3=dec2bin(GPP(ii),8);
    exm1(ii,:)=p3(5:8);
end

for ii=1:10
    ky=dec2bin(BPP(ii),8);
    fky(ii)=(ky(8));
end

EX=[exm1 exm2];

disp('Extracted Message Is:');
char(bin2dec(EX)')
    
fky

// The end

