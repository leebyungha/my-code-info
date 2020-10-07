# Prompt
> **라우터 이동 및 뒤로가기, 새로고침, 닫기 감지 코드

>> 
```javascript
// base
import React, { useEffect, useState } from 'react';
import { Prompt } from 'react-router-dom';

// modules
import { Modal } from 'antd';

const PromptInReactRouterDom = ({ history }) => {
  const [visible, setVisible] = useState(false);
  const [moveLocationPathName, setMoveLocationPathName] = useState('');

  useEffect(() => {
    window.onbeforeunload = () => { return false; };
    return () => {
      window.onbeforeunload = null;
    }
  }, []);

  return (
    <>
      <Prompt
        message={(location, action) => {
          if (action === 'PUSH') {
            setVisible(true);
            setMoveLocationPathName(location.pathname)

            return false;
          }
        }}
      />
      <Modal
        visible={visible}
        onOk={() => {
          history.replace(moveLocationPathName);
        }}
        onCancel={() => {
          setVisible(false);
          setMoveLocationPathName('');
        }}
        okText="확인"
        cancelText="취소"
        bodyStyle={{
          textAlign: 'center'
        }}
      >
        저장하지 않은 내용은 복구되지 않습니다.<br />
        <strong>페이지를 벗어나시겠습니까?</strong>
      </Modal>
    </>
  )
}

export default CustomPrompt;
```