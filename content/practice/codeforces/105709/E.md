---
title: "CF 105709E - Khu bảo tồn Finch"
description: "Chúng tôi được cung cấp một bộ sưu tập chim sẻ, ban đầu mỗi con sống trong khu bảo tồn riêng biệt. Mỗi con chim sẻ đều có một chuỗi các đặc điểm cố định, trong đó mỗi vị trí đặc điểm là một ký tự trong bảng chữ cái nhỏ từ 'a' đến 'j'."
date: "2026-06-26T08:02:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105709
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 2-12-25 Div. 2 (Beginner)"
rating: 0
weight: 105709
solve_time_s: 42
verified: true
draft: false
---

[CF 105709E - Khu bảo tồn chim sẻ](https://codeforces.com/problemset/problem/105709/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập chim sẻ, ban đầu mỗi con sống trong khu bảo tồn riêng biệt. Mỗi con chim sẻ đều có một chuỗi đặc điểm cố định, trong đó mỗi vị trí đặc điểm là một ký tự trong bảng chữ cái nhỏ từ`'a'`ĐẾN`'j'`. Theo thời gian, Darwin thực hiện các hoạt động hợp nhất các khu bảo tồn dựa trên những đặc điểm này và chúng ta phải liên tục theo dõi xem mỗi khu bảo tồn tạo thành sẽ lớn đến mức nào cũng như tổng diện tích của khu bảo tồn lớn nhất. 

Quá trình này được thúc đẩy bởi ba loại hoạt động. Một thao tác sẽ kích hoạt một vị trí đặc điểm cụ thể và từ thời điểm đó trở đi, bất kỳ hai khu bảo tồn nào có ít nhất một con chim sẻ có cùng đặc điểm ở vị trí đó đều được coi là được kết nối và hợp nhất một cách hiệu quả. Đây không phải là sự hợp nhất một lần; đó là một quy tắc toàn cầu vẫn tồn tại và tương tác với các lần kích hoạt trước đó. Một hoạt động khác yêu cầu kích thước của khu bảo tồn có chứa một loài chim sẻ cụ thể. Hoạt động cuối cùng yêu cầu quy mô của khu bảo tồn lớn nhất hiện được hình thành. 

Các ràng buộc chặt chẽ đến mức cho thấy chúng tôi không đủ khả năng để xây dựng lại kết nối từ đầu sau mỗi lần kích hoạt. Có tới 100000 con chim sẻ, 100000 đặc điểm và 100000 truy vấn, trong khi tổng kích thước đầu vào của chuỗi đặc điểm được giới hạn bởi khoảng một triệu ký tự. Bất kỳ giải pháp nào liên tục tính toán lại khả năng kết nối trên tất cả các chim sẻ trên mỗi truy vấn sẽ quá chậm, vì ngay cả việc quét tuyến tính cho mỗi truy vấn cũng đã đạt tới 10^10 thao tác. 

Một vấn đề tế nhị phát sinh từ việc kích hoạt lặp đi lặp lại cùng một chỉ số đặc điểm. Nếu một cách tiếp cận đơn giản xử lý từng lần kích hoạt một cách độc lập và chỉ tính toán lại các kết hợp cho lần kích hoạt đó, thì nó có thể vô tình bỏ sót thực tế là các lần hợp nhất trước đó đã thay đổi cấu trúc thành phần, do đó các lần kích hoạt trong tương lai phải hoạt động trên các nhóm đã được hợp nhất chứ không phải các nhóm chim sẻ ban đầu. 

Một trường hợp cạnh khác xuất phát từ các ký tự lặp lại trong một vị trí đặc điểm. Nếu nhiều chim sẻ có chung ký tự ở một vị trí, thì tất cả chúng phải kết thúc trong một thành phần được kết nối duy nhất, chứ không chỉ là sự hợp nhất theo cặp phụ thuộc vào thứ tự xử lý. Việc hợp nhất theo cặp đơn giản mà không có nhóm thích hợp có thể dẫn đến kết nối một phần hoặc bùng nổ bậc hai. 

Trong một kịch bản thất bại cụ thể, giả sử tất cả chim sẻ đều có cùng một đặc điểm`'a'`ở vị trí 1 và chúng tôi kích hoạt vị trí đó. Một cách tiếp cận ngây thơ mà mỗi cặp sẽ thực hiện khoảng n^2 thao tác, vốn đã quá chậm. Hành vi đúng là tất cả chúng sẽ trở thành một thành phần được kết nối ngay lập tức. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng trực tiếp quá trình hợp nhất. Chúng tôi duy trì các nhóm chim sẻ rõ ràng trong mỗi khu bảo tồn. Khi một vị trí đặc điểm được kích hoạt, chúng tôi sẽ quét tất cả các loài chim sẻ và nhóm những con có cùng đặc điểm ở vị trí đó lại với nhau, hợp nhất các bộ của chúng. Truy vấn loại hai sẽ đếm kích thước của tập hợp chứa chim sẻ và loại ba sẽ theo dõi kích thước tập hợp tối đa. 

Tính đúng đắn của cách tiếp cận này rất đơn giản vì nó tuân theo các quy tắc theo đúng nghĩa đen: mọi kích hoạt đều thực thi một mối quan hệ tương đương mới dựa trên một đặc điểm và các tập hợp được hợp nhất tương ứng. Vấn đề là chi phí của nó. Mỗi lần kích hoạt có thể yêu cầu quét tất cả các loài chim sẻ và có khả năng thực hiện nhiều lần hợp nhất. Với n và q lên tới 10^5, điều này dẫn đến hành vi gần như O(nq) trong trường hợp xấu nhất, điều này vượt xa khả thi. 

Quan sát chính là cấu trúc của quy trình về cơ bản là một vấn đề kết nối động trong một cấu trúc giống như lưỡng đảng: chim sẻ chỉ kết nối thông qua các giá trị đặc điểm chung ở các vị trí được kích hoạt. Mỗi con chim ban đầu bị cô lập và các cạnh dần dần lộ ra. Khi một loại cạnh được kích hoạt (chỉ số đặc điểm), tất cả các nhóm ký tự bằng nhau tại chỉ mục đó sẽ được kết nối hoàn toàn cùng một lúc. 

Điều này cho thấy chúng ta không nên coi chim sẻ một cách trực tiếp là đối tượng chính của việc sáp nhập. Thay vào đó, chúng tôi nén tác động của mỗi lần kích hoạt thành một số ít thao tác kết hợp bằng cách nhóm chim sẻ theo ký tự cho mỗi vị trí đặc điểm trước một lần, sau đó kích hoạt toàn bộ nhóm tại thời điểm truy vấn. Cấu trúc tập hợp rời rạc (DSU) trở thành công cụ tự nhiên vì chúng ta chỉ hợp nhất các thành phần và không bao giờ phân chia chúng. 

Chúng tôi xử lý trước, đối với mọi chỉ số đặc điểm và mọi ký tự, danh sách các loài chim sẻ có ký tự đó ở vị trí đó. Sau đó, khi một đặc điểm được kích hoạt, chúng tôi hợp nhất tất cả các loài chim sẻ bên trong mỗi nhóm ký tự cho đặc điểm đó. Mỗi finch tham gia vào tối đa k nhóm và mỗi nhóm chỉ được xử lý một lần, do đó tổng công việc hợp nhất sẽ trở thành tuyến tính theo nk trên tất cả các lần kích hoạt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(nk) | Quá chậm | 
| Tối ưu (DSU + nhóm) | O(nk + q α(n)) | O(nk) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biểu diễn mỗi con chim sẻ như một nút trong cấu trúc tập hợp rời rạc. Mỗi nút bắt đầu trong thành phần riêng của nó với kích thước bằng một. Chúng tôi cũng duy trì một biến toàn cục theo dõi kích thước thành phần tối đa. 

Chúng tôi tính toán trước một cấu trúc ánh xạ từng chỉ số đặc điểm và ký tự vào danh sách các loài chim sẻ có giá trị chính xác đó tại vị trí đó. Điều này biến đổi các chuỗi đầu vào thành một tập hợp các nhóm đại diện cho tất cả các hoạt động hợp nhất tiềm năng trong tương lai. 

Sau đó, chúng tôi xử lý các truy vấn theo thứ tự nhưng chúng tôi đảm bảo rằng mỗi chỉ mục đặc điểm được kích hoạt tối đa một lần. Mảng boolean theo dõi xem một đặc điểm đã được áp dụng hay chưa vì việc lặp lại nó không có tác dụng bổ sung. 

Thuật toán tiến hành như sau. 

1. Xây dựng một DSU trên n nút, khởi tạo các con trỏ cha tới chính chúng và có kích thước bằng 1. Kích thước thành phần tối đa bắt đầu từ 1. Điều này thiết lập một bất biến rằng mọi kích thước thành phần được lưu trữ trong DSU là chính xác. 
2. Tính toán trước các nhóm sao cho với mỗi vị trí đặc điểm j và mỗi ký tự c, chúng ta lưu trữ tất cả các chỉ số i trong đó finch i có ký tự c ở vị trí j. Bước này chuyển đổi kích hoạt trong tương lai thành một tập hợp các liên kết lô. 
3. Duy trì một mảng được kích hoạt[j] để theo dõi xem đặc điểm j đã được xử lý chưa. Điều này ngăn chặn công việc lặp đi lặp lại và đảm bảo mỗi nhóm được hợp nhất chính xác một lần. 
4. Đối với truy vấn loại 1 có đặc điểm j, nếu nó không được kích hoạt, hãy lặp lại tất cả các ký tự từ`'a'`ĐẾN`'j'`. Đối với mỗi nhóm ký tự ở vị trí j, liên tục kết hợp tất cả các loài chim sẻ trong nhóm đó với loài chim sẻ đầu tiên trong nhóm. Điều này tạo thành một thành phần được kết nối duy nhất cho mỗi (j, ký tự). Sau khi xử lý, đánh dấu j là đã kích hoạt. 
5. Đối với truy vấn loại 2, xuất ra kích thước của thành phần DSU chứa chim sẻ được truy vấn. 
6. Đối với truy vấn loại 3, xuất kích thước thành phần tối đa toàn cục. 

Chi tiết triển khai chính là khi xử lý một nhóm, chúng tôi không hợp nhất tất cả các cặp. Thay vào đó, chúng tôi chọn một đại diện và hợp nhất tất cả các phần tử khác vào đó, đảm bảo hoạt động tuyến tính trên mỗi nhóm. 

### Tại sao nó hoạt động 

Mỗi lần kích hoạt đưa ra các quan hệ tương đương có dạng “chim sẻ có cùng ký tự ở vị trí j thuộc một nhóm”. Các mối quan hệ này có tính bắc cầu trong mỗi lớp ký tự, vì vậy mỗi nhóm tạo thành một nhóm hoàn chỉnh trong biểu đồ cảm ứng. DSU duy trì chính xác các thành phần được kết nối khi chèn cạnh tăng dần. Vì mỗi nhóm được xử lý chính xác một lần nên mọi cạnh bắt buộc sẽ được thêm chính xác một lần và không thể xảy ra tình trạng thiếu kết nối vì bất kỳ hai chim sẻ nào phải được kết nối đều chia sẻ ít nhất một cặp kích hoạt (j, ký tự) và được hợp nhất trong nhóm đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n
        self.max_size = 1

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]
        if self.size[a] > self.max_size:
            self.max_size = self.size[a]

n, k, q = map(int, input().split())
traits = [input().strip() for _ in range(n)]

buckets = [[[] for _ in range(10)] for _ in range(k)]
for i in range(n):
    for j, ch in enumerate(traits[i]):
        buckets[j][ord(ch) - 97].append(i)

dsu = DSU(n)
used = [False] * k

for _ in range(q):
    tmp = input().split()
    t = int(tmp[0])

    if t == 1:
        j = int(tmp[1]) - 1
        if used[j]:
            continue
        used[j] = True

        for c in range(10):
            lst = buckets[j][c]
            if not lst:
                continue
            root = lst[0]
            for v in lst[1:]:
                dsu.union(root, v)

    elif t == 2:
        i = int(tmp[1]) - 1
        print(dsu.size[dsu.find(i)])

    else:
        print(dsu.max_size)
```Việc triển khai DSU sử dụng tính năng nén đường dẫn và kết hợp theo kích thước để việc hợp nhất lặp lại trên các nhóm lớn vẫn hiệu quả. Mảng nhóm nén thông tin đặc điểm để mỗi thao tác kết hợp chỉ được tạo một lần cho mỗi (vị trí, ký tự). Mảng đã sử dụng đảm bảo chúng tôi không bao giờ xử lý lại một chỉ mục đặc điểm, điều này rất cần thiết vì việc áp dụng lại nó sẽ lặp lại các kết hợp một cách không cần thiết. 

Một điểm tinh tế là việc kết hợp luôn được thực hiện tương ứng với một đại diện cố định của một nhóm. Điều này tránh hành vi bậc hai có thể phát sinh nếu chúng ta liên tục hợp nhất p tùy ý
