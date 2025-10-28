# codemagic.yaml  
workflows:  
  ios-workflow:  
    name: iOS Build  
    environment:  
      vars:  
        XCODE_WORKSPACE: "ios/App/App.xcodeproj"  
        XCODE_SCHEME: "App"  
      node: latest  
    triggering:  
      events:  
        - push  
      branch_patterns:  
        - pattern: '*'  
    scripts:  
      - name: Install npm packages  
        script: |  
          npm ci  
      - name: Add iOS platform  
        script: |  
          npx cap add ios  
      - name: Build iOS  
        script: |  
          npx cap sync ios  
          xcodebuild -project "$XCODE_WORKSPACE" \  
                     -scheme "$XCODE_SCHEME" \  
                     -configuration Release \  
                     -allowProvisioningUpdates \  
                     -archivePath $CM_BUILD_DIR/app.xcarchive \  
                     archive  
      - name: Export IPA  
        script: |  
          xcodebuild -exportArchive \  
                     -archivePath $CM_BUILD_DIR/app.xcarchive \  
                     -exportOptionsPlist /Users/builder/exportOptions.plist \  
                     -exportPath $CM_BUILD_DIR  
    artifacts:  
      - $CM_BUILD_DIR/*.ipa  
    publishing:  
      email:  
        recipients:  
          - YOUR_EMAIL@example.com   # <-- change this  
        notify:  
          success: true  
          failure: true  
