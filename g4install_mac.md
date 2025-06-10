# Install Geant4 on Mac M3 
## Tested environment (on 2025.6.10)
- Macbook Air M3, MacOS Sequoia 15.5
- Geant4 ver.11.3.2
- CMake (4.0.2), Root (6.36.00), Qt@5 (5.15.16_2) on Homebrew (latest on 2025.6.10)

## Reference 
- https://qiita.com/niikura/items/b8de01cc94f4c4ddb5c4
- https://qiita.com/oXyut/items/3962eff8f660856e7acd

## Step by step procedure
1. Install xcode
2. Install homebrew
3. Install cmake, root, qt@5 on homebrew
    ```
    % brew install cmake root qt@5
    ```
4. Download geant4 source code
    - https://geant4.web.cern.ch/download/11.3.2.html
5. Build
    ```
    ### Exit from (ana)conda environment if it is activated.
    % conda deactivate
    
    % mkdir ~/geant4/v11.3.2 
    % cd ~/geant4/v11.3.2 
    % tar xvf geant4-v11.3.2.tar.bz2
    % mkdir build
    % cd build
    % cmake -DCMAKE_INSTALL_PREFIX=~/geant4/v11.3.0/install \
     -DCMAKE_PREFIX_PATH=$(brew --prefix qt@5) \
     -DGEANT4_INSTALL_DATA=ON \
     -DGEANT4_USE_QT=ON \
     ../geant4-v11.3.2
    ...
    % make -j4
    ...
    % make install
    ```

6. Setup environment variable (create setup script file like geant4seup.sh)
    ```
    ### QT
    export PKG_CONFIG_PATH="/opt/homebrew/opt/qt@5/lib/pkgconfig"
    export PATH="/opt/homebrew/opt/qt@5/bin:$PATH"
    ### ROOT
    source $(root-config --prefix)/bin/thisroot.sh
    ### Geant4
    g4ver=11.3.2
    source  ~/geant4/v${g4ver}/install/bin/geant4.sh
    export DYLD_LIBRARY_PATH=~/geant4/v${g4ver}/install/lib:$DYLD_LIBRARY_PATH

    ```
7. Test example
    ```
    % mkdir test
    % rsync -a ~/geant4/v11.3.2/install/share/Geant4/examples/basic/B1 ./
    % mkdir B1_build
    % cd B1_build
    % cmake ../B1
    % make
    % ./exampleB1
    ...
    ```

