---
title: "CF 103590D - \u0410\u043d\u043e\u043d\u0438\u043c\u043d\u043e\u0441\u0442\u044c \u043d\u0430\u0448\u0435 \u0432\u0441\u0435"
description: "Chúng tôi được sắp xếp một hàng người, mỗi người đều có khả năng bị nhiễm bệnh hoặc khỏe mạnh, nhưng chúng tôi không bao giờ quan sát trực tiếp trạng thái cá nhân của họ. Thay vào đó, chúng tôi nhận được các báo cáo về các đoạn của đường thẳng."
date: "2026-07-02T22:55:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103590
codeforces_index: "D"
codeforces_contest_name: "RocketOlymp 2022 9 \u043a\u043b\u0430\u0441\u0441"
rating: 0
weight: 103590
solve_time_s: 33
verified: true
draft: false
---

[CF 103590D - \u0410\u043d\u043e\u043d\u0438\u043c\u043d\u043e\u0441\u0442\u044c \u043d\u0430\u0448\u0435 \u0432\u0441\u0435](https://codeforces.com/problemset/problem/103590/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được sắp xếp một hàng người, mỗi người đều có khả năng bị nhiễm bệnh hoặc khỏe mạnh, nhưng chúng tôi không bao giờ quan sát trực tiếp trạng thái cá nhân của họ. Thay vào đó, chúng tôi nhận được các báo cáo về các đoạn của đường thẳng. 

Mỗi câu lệnh loại 0 mô tả một khoảng chỉ số từ l đến r và xác nhận liệu có ít nhất một người bị nhiễm bệnh trong khoảng đó hay khoảng đó hoàn toàn sạch sẽ. Những tuyên bố này được đảm bảo phù hợp với ít nhất một nhiệm vụ ẩn của người nhiễm bệnh và người khỏe mạnh. 

Xen kẽ với những câu này là những truy vấn loại một, trong đó chúng ta được hỏi về một người cụ thể j. Đối với mỗi truy vấn như vậy, chúng ta phải xác định xem thông tin trong khoảng thời gian được thu thập đã buộc người j bị nhiễm bệnh, buộc họ phải khỏe mạnh hay để ngỏ cả hai khả năng. 

Khó khăn chính là thông tin chỉ được cung cấp ở dạng tổng hợp theo các khoảng thời gian, do đó mỗi ràng buộc hạn chế một tập hợp các phép gán nhị phân có thể có trên một mảng có độ dài n. Một truy vấn hỏi xem giá trị tại một vị trí có được cố định trên tất cả các phép gán hợp lệ hay không. 

Các ràng buộc n, q lên tới 2 lần 10^5 ngay lập tức loại trừ mọi giải pháp cố gắng duy trì hoặc tính toán lại tính nhất quán toàn cầu từ đầu cho mỗi truy vấn. Bất kỳ phương pháp nào xây dựng lại một nhiệm vụ khả thi đầy đủ hoặc thực hiện truyền bá theo truy vấn trong các khoảng thời gian sẽ quá chậm. 

Một trường hợp phức tạp phát sinh khi thông tin mâu thuẫn cục bộ nhưng nhất quán trên toàn cầu. Ví dụ: một phân đoạn có thể được biết là có chứa ít nhất một người bị nhiễm bệnh và sau đó một phân đoạn chồng chéo khác buộc sự lây nhiễm đó nằm ở một phần khác của khoảng thời gian đó. Câu trả lời đúng phụ thuộc vào việc liệu tất cả các phép gán nhất quán có đồng ý ở một vị trí duy nhất hay không, chứ không phụ thuộc vào việc liệu có tồn tại một bản tái thiết duy nhất hay không. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là duy trì tập hợp tất cả các mảng nhị phân hợp lệ có độ dài n phù hợp với các ràng buộc về khoảng. Sau mỗi truy vấn, chúng tôi sẽ kiểm tra xem vị trí j có được cố định trên tất cả chúng hay không. Điều này đúng về mặt khái niệm nhưng không thể thực hiện được vì số lượng phép gán hợp lệ là số mũ theo n và thậm chí việc biểu diễn chúng một cách ngầm định cũng trở nên khó khăn khi các ràng buộc chồng chéo một cách tùy ý. 

Một cách có cấu trúc hơn để xem vấn đề là coi mỗi ràng buộc là hạn chế các cấu hình có thể có trong một phạm vi. Tuyên bố rằng một khoảng không chứa người bị nhiễm sẽ sửa tất cả các vị trí trong khoảng đó về 0. Tuyên bố rằng một khoảng có ít nhất một người bị nhiễm bệnh sẽ loại bỏ khả năng tất cả các vị trí trong khoảng đó đều bằng 0 cùng một lúc. 

Thông tin chi tiết quan trọng là đảo ngược quan điểm: thay vì theo dõi tất cả các mảng hợp lệ, chúng tôi theo dõi những vị trí nào vẫn “chưa được xác định” theo nghĩa là chúng vẫn có thể là 0 hoặc 1 trong một số phép gán hợp lệ. Cấu trúc làm cho điều này khả thi là các ràng buộc “tất cả bằng 0” đều là các phép gán cục bộ, trong khi các ràng buộc “ít nhất một” chỉ quan trọng nếu chúng chưa được thỏa mãn bởi các ràng buộc đã biết. 

Điều này dẫn đến việc giảm ngoại tuyến tiêu chuẩn. Chúng tôi xử lý các ràng buộc và truy vấn theo thứ tự, nhưng chúng tôi duy trì một cấu trúc hỗ trợ hai thao tác một cách hiệu quả: đánh dấu các phạm vi là số 0 bắt buộc và kiểm tra xem một vị trí có bị bắt buộc phải bằng một hay không do là nhân chứng duy nhất có thể có của một số khoảng “ít nhất một” không thỏa mãn. Để hỗ trợ điều này, chúng tôi có thể sử dụng cây phân đoạn để duy trì cho từng phân đoạn cho dù phân đoạn đó hoàn toàn bị ép buộc bằng 0 và chúng tôi cũng duy trì số lượng phạm vi bao phủ của các ràng buộc “ít nhất một” đang hoạt động.

Ý tưởng thiết yếu là một vị trí buộc phải bằng một nếu nó được yêu cầu phải thỏa mãn một khoảng nào đó mà mọi vị trí khác đã bị buộc về 0. Điều này chuyển đổi câu hỏi về tính nhất quán toàn cục thành một truy vấn động “nhân chứng duy nhất” theo từng khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force qua bài tập | hàm mũ | hàm mũ | Quá chậm | 
| Cây phân đoạn có theo dõi ràng buộc | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc bổ sung. Một theo dõi các vị trí bị buộc về 0 theo khoảng thời gian “không lây nhiễm”. Phần còn lại theo dõi xem khoảng thời gian “ít nhất một lần lây nhiễm” vẫn có thể được thỏa mãn như thế nào. 

Đối với từng vị trí, chúng tôi cũng duy trì xem vị trí đó có còn khả năng được sử dụng làm nhân chứng trong một khoảng thời gian hoạt động nào đó hay không. 

### Các bước 

1. Khởi tạo một mảng is_zero có độ dài n, ban đầu tất cả đều sai, nghĩa là không có vị trí nào bị buộc phải lành mạnh. 
2. Duy trì cây phân đoạn hỗ trợ cập nhật phạm vi đánh dấu các vị trí là điểm 0 bắt buộc và các truy vấn điểm hỏi xem một vị trí đã bị buộc phải là 0 hay chưa. Điều này đảm bảo chúng tôi có thể nhanh chóng loại bỏ các vị trí không thể ngăn chặn sự lây nhiễm. 
3. Duy trì một danh sách hoặc cấu trúc của tất cả các ràng buộc “ít nhất một người bị nhiễm [l, r]”. Mỗi ràng buộc như vậy đang hoạt động và được coi là không thỏa mãn nếu tất cả các vị trí trong phạm vi của nó hiện bị buộc bằng 0. 
4. Khi xử lý truy vấn loại 0 với x = 0, chúng tôi đánh dấu toàn bộ khoảng [l, r] là số 0 bắt buộc trong cây phân đoạn. Điều này ngay lập tức loại bỏ tất cả khả năng lây nhiễm khỏi khu vực đó. 
5. Khi xử lý truy vấn loại 0 với x = 1, chúng tôi đăng ký khoảng này dưới dạng ràng buộc trực tiếp yêu cầu ít nhất một vị trí khác 0 bên trong nó. Ràng buộc này vẫn phù hợp cho đến khi nó được thỏa mãn bởi ít nhất một vị trí không bị ép bằng 0. 
6. Sau mỗi lần cập nhật, chúng tôi duy trì cho mỗi khoảng thời gian hoạt động “ít nhất một” xem nó đã có vị trí ứng viên chưa bị buộc phải bằng 0 hay không. Điều này có thể được theo dõi bằng cách sử dụng các truy vấn cây phân đoạn để kiểm tra xem một phân đoạn có hoàn toàn bằng 0 hay không. 
7. Đối với truy vấn loại một hỏi về vị trí j, chúng ta kiểm tra hai điều kiện. Nếu is_zero[j] đúng thì j chắc chắn khỏe mạnh. Ngược lại, chúng ta kiểm tra xem j có phải là vị trí khác 0 duy nhất có thể có trong bất kỳ khoảng hoạt động nào vẫn chưa được thỏa mãn hay không. Nếu tồn tại một khoảng trong đó j là ứng cử viên duy nhất còn lại thì j phải bị nhiễm. 
8. Nếu không áp dụng điều kiện bắt buộc bằng 0 hay bắt buộc một, câu trả lời sẽ không rõ ràng. 

### Tại sao nó hoạt động 

Thuật toán dựa vào bất biến rằng mọi ràng buộc loại x = 0 sẽ loại bỏ vĩnh viễn tất cả các khả năng lây nhiễm trong khoảng của nó và mọi ràng buộc loại x = 1 phải được hỗ trợ bởi ít nhất một vị trí ứng cử viên còn sống sót trong khoảng của nó. Bất kỳ vị trí nào là ứng cử viên duy nhất còn lại cho một số ràng buộc hoạt động sẽ bị buộc phải đáp ứng ràng buộc đó về mặt logic. Vì tất cả các ràng buộc đều đơn điệu theo nghĩa là việc loại bỏ các ứng cử viên không bao giờ làm mất hiệu lực tính nhất quán, nên một khi một vị trí trở nên cần thiết duy nhất thì nó vẫn giữ nguyên như vậy trong tất cả các lần hoàn thành hợp lệ. Điều này đảm bảo rằng các nhiệm vụ bắt buộc phải nhất quán trên tất cả các cấu hình đáp ứng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = n
        self.t = [0] * (4 * n)
        self.lazy = [0] * (4 * n)

    def push(self, v, l, r):
        if self.lazy[v]:
            self.t[v] = 1
            if l != r:
                self.lazy[v*2] = 1
                self.lazy[v*2+1] = 1
            self.lazy[v] = 0

    def update(self, v,
```
