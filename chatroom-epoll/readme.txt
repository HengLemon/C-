�������ˣ�

1.socket()��������Socket
2.bind()�󶨷������˿�
3.listen()�����ͻ�������
4.accept()��������
5.recv/send���ռ���������
6.close()�ر�socket

�ͻ��ˣ�

1.socket()��������Socket
2.connect()���ӷ�����
3.recv/send���ռ���������
4.close()�ر�socket

�������ķ�����������Ҫ����Ľӿڣ�

1.��ʼ�� Init()
2.�رշ��� Close()
3.�������� Start()
4.�㲥��Ϣ�����пͻ��� SendBoradcastMessage()

1.��ʼ�� Init()
// ��ʼ������˲���������
void Server::Init() {

    // Step 1����������socket
    // ʹ��socket()

    // Step 2���󶨵�ַ
    // ʹ��bind()

    // Step 3����������
    // ʹ��listen()

    // Step 4�������¼���
    // epoll_create()

    // Step 5����Ӽ���fd��epoll fd
    // addfd()
}


2.Close()
�ر����д򿪵��ļ�������

3.�������� Start()

// ���������
void Server::Start() {
    // Step 1����ʼ�������
    // Init()

    // Step 2��������ѭ��

    // Step 3����ȡ�������¼�
    // epoll_wait()

    // Step 4��ѭ���������о������¼�
    // 4.1 �������������������Ӳ���������ӵ�epoll fd
    // accept() addfd()

    // 4.2 ���������Ϣ��㲥�������ͻ���
    // SendBroadcastMessage
}


4.�㲥��Ϣ�����пͻ��� SendBoradcastMessage()

// ���͹㲥��Ϣ�����пͻ���
int Server::SendBroadcastMessage(int clientfd) {
    // Step 1����������Ϣ
    // recv()

    // Step 2���ж��Ƿ��ǿͻ�����ֹ����

    // Step 3���ж��Ƿ������һ��������ͻ���

    // Step 4����ʽ�����͵���Ϣ����
    // sprintf()

    // Step 5�������ͻ����б����η�����Ϣ
    // send()
}




�ͻ����������������Ҫ����Ľӿڣ�

1.���ӷ����Connect()
2.�˳�����Close()
3.�����ͻ���Start()


1.���ӷ����Connect()
// ���ӷ�����
void Server::Connect() {

    // Step 1������socket
    // ʹ��socket()

    // Step 2�����ӷ����
    // connect()

    // Step 3�������ܵ�������fd[0]���ڸ����̶���fd[1]�����ӽ���д
    // ʹ��pipe()

    // Step 4������epoll
    // epoll_create()

    // Step 5����sock�͹ܵ���������������ӵ��ں��¼�����
    // addfd()
}

2.�˳�����Close()

��Ҫ�ر����д򿪵��ļ��������Ϳ��ԡ�

��Ҫע���ж����ڸ����̻����ӽ��̣��ͻ����е��������̴򿪵Ĺܵ��ļ��������ǲ�ͬ�ġ�


3.�����ͻ���Start()

// �����ͻ���
void Client::Start() {
    // Step 1�����ӷ�����
    // Connect()

    // Step 2�������ӽ���
    // fork()

    // Step 3�������ӽ���ִ������
    // �ӽ��̸����ռ��û��������Ϣ��д��ܵ�
    // fgets() write(pipe_fd[1])

    // Step 4�����븸����ִ������
    // �����̸�����ܵ����ݼ�epoll�¼�
    // 4.1 ��ȡ�����¼�
    // epoll_wait()
    // 4.2 ��������¼�
    // ���շ���������Ϣ����ʾ recv()
    // ��ȡ�ܵ���Ϣ����������� read() send()
}