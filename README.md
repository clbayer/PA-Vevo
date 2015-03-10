# PA-Vevo
Export PA data from Vevo to Matlab

% Program to extract PA data from Vevo exported files
% V1 Oct 2013 

close all
clear all
clc

%/// user-defined \\\
ModeName = '.pamode';

files = dir('*.raw.xml');   %save name of all files ending in .raw.xml in current directory to 'files'
NFiles = size(files,1);
for ifile = 1:NFiles   %loop through all files
    
    %Vevo aquisition mode
    fnameBase = files(ifile).name(1:end-4);
    fnameXml = [fnameBase '.xml'];
    param = VsiParseXml(fnameXml, ModeName);
    %PaScanDistance = param.PaScanDistance; %mm
    %PaStepSize = param.PaStepSize; %mm
    %NFrames = round(PaScanDistance/PaStepSize);
    NFrames = 5;
    NWavelength = 1;    % temporary for debugging
    
    fnameBase =  files(ifile).name(1:end-4);
    for iframe = 1:NFrames
            [Rawdata, WidthAxis, DepthAxis] = VsiOpenRawPa(fnameBase, ModeName, iframe);     
            PA(:,:,1,iframe) = Rawdata(1:end-10,:); 
%            for iwl = 1:NWavelength
%             fnameBase =  files(ifile).name(1:end-4);
%             [Rawdata, WidthAxis, DepthAxis] = VsiOpenRawPa(fnameBase, ModeName, iframe);
%                      
%             PA(:,:,mod(iframe-1,NWavelength)+(iwl-1)*NWavelength+1, ceil(iframe/NWavelength)) = Rawdata(1:end-10,:); % mod finds remainder of division by 5 to separate data into wavelengths; ceil rounds up to find frame
%         end
    end
    
    
    fnamesave = files(ifile).name(1:end-8);
    save([num2str(fnamesave) '_sPA'],'PA') 
    clear('PA');
end
    
  
    
