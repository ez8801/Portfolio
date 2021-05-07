![ThumbNail](https://media-exp1.licdn.com/dms/image/C5103AQEaUrgBF7L11Q/profile-displayphoto-shrink_200_200/0/1516934464463?e=1625702400&v=beta&t=y3EmG0vFd04BBAtEjwWtAYAACzvZ34lG182QhiMxweo)

개발팀의 Yes Man, 백민우입니다.

# About Me
### Introduction
- 안녕하세요! 모바일 게임 클라이언트 개발자 백민우입니다.
- 원칙과 직관을 따르는 **열정적인 중재자**(INFP-T)입니다.

### Contact & Channel
- Email | usd122@naver.com
- Github | https://github.com/ez8801
- Linkedin | https://www.linkedin.com/in/minwoo-baek-b9476756

# Skills
### Game Client
- C#, C++
- Unity3d
- WinForms

### DevOps
- Jenkins

### Native
- Java, Objective-C
- Android, iOS

### Collaboration
- Trello, Asana
- REDMINE, mantis
- JANDI, Telegram

### Version Control
- SVN

# Work Experience & Projects
### 게임빌 프로야구
- Unity AssetBundle, Android Expansion File (*.obb) 등을 이용 패치 시스템 구현
- Android App Bundle (*.aab), 에디터 및 OS 업데이트 대응
- 패치 시스템에서부터 지속적 통합/배포 (CD/CI)를 자기학습하여 팀에 전파
![](https://github.com/ez8801/Portfolio/blob/main/img/jenkins_01.PNG)


- 각종 컨텐츠 구현

### The King Of Fighters All Stars
- 컨텐츠 제작
![] ()

- Utils, Extension 제작

### Knights of night
- 각종 컨텐츠 구현
![](https://github.com/ez8801/Portfolio/blob/main/img/kon_01.png)

- 밸런스 툴 제작
- 넷마블 메모리 보안 모듈 적용
- Debug Monitor

![](/img/tool_console.png)

- 밸런스 툴 제작

![](/blob/main/img/tool_balance.png)
 
# Personal Experience & Projects
##### 어플리케이션 제작 및 T store 배포
![](https://github.com/ez8801/Portfolio/blob/main/img/personal_exp_01.png)

##### 학생회 홈페이지 제작

![](https://github.com/ez8801/Portfolio/blob/main/img/personal_exp_02.png)

node.js, EXPRESS, Jade 활용


# Education
#### 한양대학교 응용시스템학과
- 3학년 과대표 1년 역임
#### 한국 콘텐츠 진흥원 - 차세대 게임 과정
- 1년 교육 과정 수료
- 성적 우수 국비 장학생 선발 카네기멜론대학 ETC 센터 연수 (2개월)

# Certificate
### 정보처리 기능사
* 2011.03.07 취득

## CD/CI

#### [JANDI](https://www.jandi.com/landing/) 커넥트 Incoming Webhook으로 외부 데이터를 잔디 메시지로 수신하기

메신저로 빌드 진행상황을 수신, 대응

```sh
curl
-X POST $JENKINS_WEB_HOOK_URL
-H "Accept: application/vnd.tosslab.jandi-v2+json"
-H "Content-Type: application/json"
--data-binary "{\"body\":\"$JOB_NAME v${APP_VERSION} r${REVISION} Build Started. \"}"
```

![](https://github.com/ez8801/Portfolio/blob/main/img/jenkins_webhook_01.PNG)

> Note: 젠킨스 원격 빌드을 이용하면 메신저에서 클릭 한번 만으로 후속조치(ex) 배포)를 할 수 있다. 
> 
> (깃털만큼의 귀찮음이라도 덜고 눈 깜박할 시간이라도 퇴근을 앞당기도록 하자.)

#### 플러그인, Shell 그리고 Unity 까지

Unity3d를 batchmode로 실행, 인자를 전달하여 옵션을 조절할 수 있다!

```sh
-quit -batchmode -projectPath "$WORKSPACE" -logFile "$WORKSPACE/log.log" -executeMethod ProjectBuilder.Build() -version $APP_VERSION -revision $REVISION -symbols $DEFINE_SYMBOLS
```

대부분의 설정은  유지 일부 설정은 

e.g) Android Extension File(*.obb)에 사용되는 unity.build-id 값

```
[UnityEditor.MenuItem("Build/Restore Unity Build Id")]
private static void RestoreUnityBuildId()
{
    string path = "AndroidManifest File Path";
    if (false == File.Exists(path))
        throw new FileNotFoundException("Android Manifest File");
        
    string idFilePath = "Id file Path";
    if (false == File.Exists(idFilePath))
        throw new FileNotFoundException("Id file Path");
        
    string unityBuildId = File.ReadAllText(idFilePath);
    XmlDocument doc = new XmlDocument();
    doc.Load(path);
    
    XmlNode manifestNode = doc.FindChildNode("manifest");
    XmlNode applicationNode = manifestNode.FindChildNode("application");
    
    string ns = applicationNode.GetNamespaceOfPrefix("android");
    
    // unity.build-id
    XmlElement element = applicationNode.FindElementWithAndroidName("meta-data", "name", ns, "unity.build-id");
    if (element == null)
    {
        element = doc.CreateElement("meta-data");
        element.SetAttribute("name", ns, "unity.build-id");
        element.SetAttribute("value", ns, unityBuildId);
        manifestNode.AppendChild(element);
    }
    else
    {
        string value = element.GetAttribute("value", ns);
        element.SetAttribute("value", ns, unityBuildId);
    }
    
    XmlWriterSettings settings = new XmlWriterSettings
    {
        Indent = true,
        IndentChars = "  ",
        NewLineChars = System.Environment.NewLine,
        NewLineHandling = NewLineHandling.Replace
    };
    
    using (XmlWriter xmlWriter = XmlWriter.Create(path, settings))
    {
        doc.Save(xmlWriter);
    }
}
```

#### Shell

쉘이랑 Apk뽑고, 사이닝도하고 백업도 다 했어!

```sh
## Appguard
java -jar appguard-cli-builder.jar -h -i ${UNSIGNED_APK_NAME}

## Sign
jarsigner -verbose -tsa http://timestamp.comodoca.com/rfc3161 -sigalg SHA1withRSA -digestalg SHA1 -keystore ${KEYSTORE_NAME} ${APPGUARD_APK_NAME} ${ALIAS_NAME} -storepass ${STORE_PASS} -keypass ${KEY_PASS}

## Zip align
./zipalign -v 4 ${APPGUARD_APK_NAME} ${WORKSPACE}/OUTPUT/APK/${APK_NAME}

## Verify
cd ${WORKSPACE}/OUTPUT/APK
jarsigner -verify -verbose -certs ${APK_NAME}

## Archive
mkdir -p ${JENKINS_HOME}/jobs/$JOB_NAME/builds/${BUILD_NUMBER}/archive
cp ${APK_NAME} ${JENKINS_HOME}/jobs/$JOB_NAME/builds/${BUILD_NUMBER}/archive/${APK_NAME}
```


#### 엑셀에서 바이너리 데이터로

특정 키로 추출된 다이제스트를 파일 끝에 붙여 파일 변조 여부를 판단

```
// Export
public void ExportBytes(string bytesFilePath)
{
    using (BinaryWriter bw = new BinaryWriter(new FileStream(bytesFilePath, FileMode.Create), Encoding.UTF8))
    {
        // Write Header
        // ...
        // Write Body Data
        // ...
        
        // Generate Key
        string key = Data.Crypto.GetRandomKey();
        byte[] keyBytes = Encoding.UTF8.GetBytes(key);

        // Write Key Size
        bw.Write((short)keyBytes.Length);

        // Write Key Bytes
        bw.Write(keyBytes);

        HMACMD5 oHMACMD5 = new HMACMD5(keyBytes);
        byte[] hashValue = oHMACMD5.ComputeHash(bytes);

        // Write Digest
        bw.Write(hashValue);
    }
}

...

// Load
public abstract class Table<T> : ITable where T : IDeserializable, new()
{
    ...
    public void Load(TextAsset textAsset)
    {
        if (textAsset == null)
            throw new System.ArgumentNullException("textAsset");

        Clear();
        Name = textAsset.name;

        Deserializer deserializer = new Deserializer();
        deserializer.ReadHeader(textAsset);
        EnsureCapacity(deserializer.RowCount);
        Deserialize(0, deserializer.RowCount, deserializer);
        deserializer.Validate();
        OnFinishedLoad();
    }
    ...
}

// Verity
private bool IsValid()
{
    byte[] keyBytes = null;
        
    //...
    // keyBytes = GetKeyBytes();
    //...

    HMACMD5 hmaCMD5 = new HMACMD5(keyBytes);

    //...

    byte[] computedHash = hmaCMD5.ComputeHash(data);
    if (IsMatch(ref computedHash, ref digest))
        return true;
}
```


#### Bundle Manifest Window

![](https://github.com/ez8801/Portfolio/blob/main/img/AssetBundleManifest.PNG)

#### Dependency Tool

![](https://github.com/ez8801/Portfolio/blob/main/img/DependencyTools.png)
