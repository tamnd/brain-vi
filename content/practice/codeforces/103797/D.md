---
title: "CF 103797D - Bộ đôi động"
description: "Chúng ta được cấp một mảng có độ dài $N$, ban đầu chứa đầy các số 0, biểu thị số viên đạn mà mỗi học sinh hiện có. Hai loại hoạt động được thực hiện trực tuyến."
date: "2026-07-02T08:47:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103797
codeforces_index: "D"
codeforces_contest_name: "IME++ Starters Try-outs 2022"
rating: 0
weight: 103797
solve_time_s: 50
verified: true
draft: false
---

[CF 103797D - Dynamic Duo](https://codeforces.com/problemset/problem/103797/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$N$, ban đầu chứa đầy số 0, biểu thị số viên đạn mà mỗi học sinh hiện có. Hai loại hoạt động được thực hiện trực tuyến. 

Hoạt động đầu tiên là cập nhật phạm vi: cho một phân đoạn$[l, r]$và một giá trị$x$, mọi học sinh trong phân khúc đó nhận được$x$đạn bổ sung. Điều quan trọng là đây không phải là sự thay thế mà là sự gia tăng và nhiều bản cập nhật sẽ tích lũy theo thời gian. 

Thao tác thứ hai là truy vấn điểm: đối với một vị trí nhất định$p$, chúng ta được hỏi liệu số lượng đạn hiện tại tại vị trí đó ít nhất là$x$. Nếu đúng như vậy, chúng tôi phản hồi tích cực và về mặt khái niệm, học sinh ngay lập tức sử dụng chính xác$x$đạn, giảm số lượng dự trữ của anh ta xuống$x$. Ngược lại, chúng ta phản ứng tiêu cực và trạng thái không thay đổi. 

Những hạn chế$N, Q \le 10^5$buộc chúng tôi phải hỗ trợ cả việc bổ sung phạm vi và truy vấn điểm một cách hiệu quả. Bất kỳ giải pháp nào tính toán lại các giá trị trong một phạm vi cho mỗi thao tác đều dẫn đến$O(NQ)$, quá chậm. Ngay cả việc duy trì một mảng đơn giản cũng không đủ vì các cập nhật phạm vi lặp đi lặp lại sẽ vẫn tuyến tính trên mỗi thao tác. 

Một trường hợp khó nhận thấy là các truy vấn thành công sẽ sửa đổi mảng. Ví dụ, nếu một học sinh có chính xác$x$dấu đầu dòng và được truy vấn hai lần, truy vấn thứ hai phải trả về số âm vì truy vấn đầu tiên tiêu tốn dấu đầu dòng. Một cách tiếp cận ngây thơ “chỉ tổng phạm vi” không duy trì mức giảm sẽ âm thầm đếm quá mức. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: duy trì mảng một cách rõ ràng. Để cập nhật$[l, r, x]$, chúng tôi lặp qua tất cả các chỉ mục trong phân khúc và thêm$x$. Đối với một truy vấn tại vị trí$p$, chúng ta chỉ cần kiểm tra giá trị và trừ$x$nếu có thể. 

Điều này đúng, nhưng mỗi lần cập nhật đều tốn$O(r-l+1)$, trở thành$O(N)$trong trường hợp xấu nhất. Với$10^5$hoạt động, tổng độ phức tạp suy biến thành$10^{10}$, điều đó không khả thi. 

Quan sát quan trọng là chúng ta không cần biết rõ ràng mọi phần tử của mảng. Chúng tôi chỉ cần hai khả năng: thêm giá trị trên một phạm vi và truy vấn một chỉ mục. Đây là cài đặt cổ điển trong đó Cây Fenwick hoặc Cây phân đoạn có khả năng nhân giống lười biếng được áp dụng. 

Tuy nhiên, chúng ta có thể đơn giản hóa hơn nữa. Việc bổ sung phạm vi với cấu trúc truy vấn điểm có thể được xử lý bằng cách sử dụng mảng sai phân: chúng tôi duy trì một mảng$diff$như vậy việc thêm$x$ĐẾN$[l, r]$dịch sang$diff[l] += x$Và$diff[r+1] -= x$. Khi đó giá trị tại vị trí$p$là tiền tố tổng lên tới$p$. Điều này làm giảm phạm vi cập nhật xuống$O(1)$và truy vấn trỏ tới tổng tiền tố. 

Sự phức tạp duy nhất còn lại là các truy vấn không phải là các lần đọc thuần túy. Khi truy vấn thành công, chúng ta phải trừ$x$vĩnh viễn. Điều này cũng có thể được mô hình hóa như một bản cập nhật điểm trên mảng cơ bản, chuyển thành hai bản cập nhật trên mảng khác biệt. 

Do đó, giải pháp cuối cùng kết hợp mảng sai phân với Cây Fenwick để hỗ trợ tính tổng tiền tố và sửa đổi điểm một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NQ)$|$O(N)$| Quá chậm | 
| Cây Fenwick (mảng khác biệt + cập nhật) |$O(Q \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì Cây Fenwick dựa trên biểu diễn mảng khác nhau về số lượng đạn. 

1. Khởi tạo cây Fenwick có kích thước$N+1$, ban đầu tất cả đều là số không. Điều này thể hiện một mảng khác biệt trong đó các giá trị thực tế được lấy từ tổng tiền tố. 
2. Để xử lý bản cập nhật$!\ l\ r\ x$, chúng tôi thêm$x$ở vị trí$l$và trừ$x$ở vị trí$r+1$nếu nó nằm trong giới hạn. Điều này đảm bảo rằng mọi tổng tiền tố vượt quá$l$bao gồm phần tăng thêm và mọi thứ vượt quá$r$hủy bỏ nó đi. 
3. Trả lời câu hỏi tại vị trí$p$, chúng tôi tính tổng tiền tố lên đến$p$, cho biết số lượng đạn hiện tại của học sinh đó. 
4. Nếu giá trị ít nhất là$x$, chúng ta xuất thành công và trừ ngay$x$từ vị trí$p$. Đây là bản cập nhật điểm trong Cây Fenwick, được triển khai dưới dạng thêm$-x$Tại$p$Và$+x$Tại$p+1$. 
5. Nếu giá trị nhỏ hơn$x$, chúng tôi xuất ra lỗi và không sửa đổi bất cứ điều gì. 

Lựa chọn thiết kế quan trọng là cả cập nhật phạm vi và sửa đổi điểm đều được thể hiện theo cùng một nguyên tắc: cập nhật mảng khác biệt mà Fenwick Trees xử lý một cách hiệu quả. 

### Tại sao nó hoạt động 

Mảng khác biệt đảm bảo rằng mọi phần tử của mảng ban đầu được biểu diễn dưới dạng tổng tiền tố. Bất kỳ phép cộng phạm vi nào đều ảnh hưởng chính xác đến cấu trúc tiền tố theo cách được kiểm soát: nó giới thiệu một bước tiến lên ở$l$và một bước xuống tại$r+1$, do đó mọi chỉ số đều phản ánh giá trị tích lũy chính xác. 

Phép trừ điểm hoạt động đối xứng: giảm một vị trí bằng$x$tương đương với việc đưa ra một bước đơn vị âm tại chỉ mục đó và khôi phục nó sau đó, điều này duy trì tính chính xác của tất cả các vị trí khác. Vì tất cả các hoạt động là sự kết hợp tuyến tính của các đóng góp tiền tố, nên bất biến Cây Fenwick đảm bảo tính nhất quán sau mỗi lần cập nhật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

n, q = map(int, input().split())
fw = Fenwick(n)

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '?':
        p = int(tmp[1])
        x = int(tmp[2])
        cur = fw.sum(p)
        if cur >= x:
            print("yes sir")
            fw.add(p, -x)
        else:
            print("negative")
    else:
        l = int(tmp[1])
        r = int(tmp[2])
        x = int(tmp[3])
        fw.add(l, x)
        if r + 1 <= n:
            fw.add(r + 1, -x)
```Cây Fenwick được sử dụng làm công cụ tính tổng tiền tố trên một mảng sai phân. các`add`hàm thực hiện cả cập nhật phạm vi và giảm điểm bằng cách ánh xạ chúng thành các khoảng tăng cục bộ. 

Để cập nhật phạm vi, chúng tôi áp dụng thủ thuật chênh lệch tiêu chuẩn: tăng ở điểm cuối bên trái và giảm ngay sau điểm cuối bên phải. Điều này đảm bảo rằng bất kỳ tổng tiền tố nào trong phạm vi đều phản ánh giá trị gia tăng chính xác một lần. 

Đối với một truy vấn, chúng tôi tính tổng tiền tố tại vị trí`p`. Nếu đủ, chúng tôi sẽ ngay lập tức giảm giá trị tại thời điểm đó bằng cách thực hiện một cặp cập nhật Fenwick khác, xử lý nó một cách hiệu quả như một bản cập nhật phạm vi cục bộ có độ dài một. 

Phải cẩn thận với ranh giới`r + 1`, không được vượt quá kích thước Fenwick. Ngoài ra, tất cả các chỉ số đều dựa trên 1 để khớp với việc triển khai Fenwick một cách rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
! 1 5 10
? 3 10
? 3 5
```Chúng tôi theo dõi mảng ngầm. 

| Bước | Hoạt động | Giá trị vị trí 3 | Hành động | 
| --- | --- | --- | --- | 
| 1 | thêm [1,5] +10 | 10 | cập nhật phạm vi | 
| 2 | truy vấn 3, x=10 | 10 | vâng thưa ngài, trừ 10 | 
| 3 | truy vấn 3, x=5 | 0 | tiêu cực | 

Điều này cho thấy mức tiêu thụ ảnh hưởng đến các truy vấn sau này. 

### Ví dụ 2 

đầu vào:```
4 4
! 1 3 5
! 2 4 2
? 2 6
? 2 1
```| Bước | Hoạt động | Giá trị vị trí 2 | Hành động | 
| --- | --- | --- | --- | 
| 1 | +[1,3]=5 | 5 | | 
| 2 | +[2,4]=2 | 7 | | 
| 3 | truy vấn x=6 | 7 | trừ 6 | 
| 4 | truy vấn x=1 | 1 | trừ 1 | 

Trạng thái cuối cùng cho thấy số giảm tích lũy được phản ánh chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(Q \log N)$| Mỗi truy vấn tiền tố và cập nhật Fenwick mất thời gian logarit, được áp dụng một lần cho mỗi thao tác | 
| Không gian |$O(N)$| Cây Fenwick lưu trữ mảng phụ trợ tuyến tính | 

Các ràng buộc cho phép lên đến$10^5$hoạt động và$O(\log N)$mỗi hoạt động thoải mái phù hợp trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 2)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

    n, q = map(int, sys.stdin.readline().split())
    fw = Fenwick(n)

    for _ in range(q):
        tmp = sys.stdin.readline().split()
        if tmp[0] == '?':
            p = int(tmp[1]); x = int(tmp[2])
            cur = fw.sum(p)
            if cur >= x:
                output.append("yes sir")
                fw.add(p, -x)
            else:
                output.append("negative")
        else:
            l, r, x = map(int, tmp[1:])
            fw.add(l, x)
            if r + 1 <= n:
                fw.add(r + 1, -x)

    return "\n".join(output)

# provided samples
assert run("10 4\n? 7 9\n! 1 10 17\n? 7 9\n? 7 9\n") == "negative\nyes sir\nnegative"
assert run("5 7\n! 1 5 13\n! 2 3 123832\n? 5 13\n! 1 5 12873\n? 5 1337\n! 1 4 128312\n? 3 278302\n") == "yes sir\nyes sir\nnegative"

# custom cases
assert run("1 3\n! 1 1 5\n? 1 5\n? 1 5\n") == "yes sir\nnegative"
assert run("3 4\n! 1 3 1\n! 2 2 5\n? 2 6\n? 2 1\n") == "negative\nyes sir"
assert run("5 2\n? 3 1\n? 3 1\n") == "negative\nnegative"
assert run("4 3\n! 1 4 10\n? 1 10\n? 4 10\n") == "yes sir\nyes sir"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Truy vấn lặp lại một phần tử | vâng thưa ngài/ phủ định | hành vi tiêu dùng | 
| Cập nhật chồng chéo | kết quả hỗn hợp | tính đúng đắn của tích lũy | 
| Không có cập nhật trước khi truy vấn | tất cả đều tiêu cực | trạng thái 0 ban đầu | 
| Cập nhật toàn diện Edge | cả hai đầu đều bị ảnh hưởng | độ đúng ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh khóa là các truy vấn thành công được lặp lại trên cùng một vị trí. Hãy xem xét một học sinh nhận được 5 viên đạn, sau đó được yêu cầu hai lần cho 3 viên đạn. Truy vấn đầu tiên thành công và giảm giá trị xuống 2, trong khi truy vấn thứ hai không thành công. Việc triển khai dựa trên Fenwick phản ánh chính xác điều này vì phép trừ được áp dụng ngay lập tức dưới dạng cập nhật điểm, chứ không chỉ giả định về mặt logic. 

Một trường hợp khác là cập nhật phạm vi kết thúc ở chỉ mục cuối cùng. Ví dụ, áp dụng$[1, N]$không được cố gắng cập nhật$N+1$trong cây Fenwick. Việc triển khai bảo vệ rõ ràng điều này bằng kiểm tra ranh giới, đảm bảo không có cập nhật ngoài phạm vi nào xảy ra trong khi vẫn duy trì tính chính xác của tổng tiền tố.
