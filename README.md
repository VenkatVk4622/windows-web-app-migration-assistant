Resources:
  Ec2Instance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: ami-0453ec754f44f9a4a
      InstanceType: t3.medium
      Tags:
        - Key: Name
          Value: "WWAMA Lab"
      SecurityGroups:
        - !Ref MySecurityGroup
      KeyName:
        Ref: "KeyPair"
      UserData:
        !Base64 |
          <powershell>
          (new-object net.webclient).DownloadFile('https://github.com/awslabs/windows-web-app-migration-assistant/archive/master.zip','c:\wwama.zip')
          </powershell>
  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupName: SimpleWinGroup
      GroupDescription: Enable RDP traffic via port 3389
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 3389
          ToPort: 3389
          CidrIp: 0.0.0.0/0
Parameters:
  KeyPair:
    Description: Key Pair for this Instance
    Type: "AWS::EC2::KeyPair::KeyName"
Outputs:
  ServerDns:
    Value: !GetAtt
      - Ec2Instance
      - PublicDnsName
