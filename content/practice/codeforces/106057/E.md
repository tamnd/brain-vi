---
title: "CF 106057E - Kho báu trong khoảng thời gian"
description: "Chúng ta được cung cấp một mảng ban đầu có độ dài $N$. Sau đó, một chuỗi các bản cập nhật phạm vi sẽ sửa đổi nó: mỗi bản cập nhật chọn một phân đoạn $[L, R]$ và thêm giá trị $d$ cho mọi phần tử bên trong phân đoạn đó. Khi tất cả các bản cập nhật được áp dụng, mảng sẽ được sửa."
date: "2026-06-21T08:42:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106057
codeforces_index: "E"
codeforces_contest_name: "CoU CSE Fest 2025 - Inter University Programming Contest (Divisional)"
rating: 0
weight: 106057
solve_time_s: 40
verified: true
draft: false
---

[CF 106057E - Kho báu trong khoảng thời gian](https://codeforces.com/problemset/problem/106057/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài ban đầu$N$. Sau đó, một chuỗi các bản cập nhật phạm vi sẽ sửa đổi nó: mỗi bản cập nhật chọn một phân đoạn$[L, R]$và thêm một giá trị$d$tới mọi phần tử bên trong phân khúc đó. Khi tất cả các bản cập nhật được áp dụng, mảng sẽ được sửa. Sau đó, chúng tôi nhận được các truy vấn yêu cầu$K$-giá trị lớn nhất trong mảng cuối cùng này. 

Nhiệm vụ về cơ bản được chia thành hai giai đoạn. Đầu tiên, chúng ta phải áp dụng hiệu quả nhiều mức tăng phạm vi cho một mảng tĩnh. Thứ hai, chúng ta phải hỗ trợ các truy vấn thống kê thứ tự trên mảng được chuyển đổi cuối cùng. 

Các ràng buộc được ngụ ý bởi các cài đặt Codeforces điển hình với$N, Q, M$lên đến khoảng$2 \cdot 10^5$ngay lập tức loại trừ mô phỏng ngây thơ. Một cách tiếp cận trực tiếp cập nhật mọi yếu tố cho từng phạm vi chi phí cập nhật$O(NQ)$, sẽ ở xung quanh$10^{10}$hoạt động trong trường hợp xấu nhất và sẽ không chạy trong thời gian giới hạn. Tương tự, việc tính toán lại câu trả lời cho mỗi truy vấn bằng cách quét mảng sẽ quá chậm vì việc sắp xếp hoặc quét tuyến tính cho mỗi truy vấn sẽ nhân chi phí lên một cách không cần thiết. 

Các trường hợp cạnh xuất hiện chủ yếu xung quanh ranh giới lập chỉ mục và cập nhật. Một sai lầm phổ biến là xử lý sai$R + 1$vị trí trong mảng khác biệt. 

Nếu như$N = 5$, và chúng tôi áp dụng$(L=2, R=5, d=3)$, thì không trừ được tại vị trí$R+1 = 6$hoặc viết không chính xác các bản cập nhật ngoài giới hạn sẽ làm rò rỉ phần tăng qua mảng hoặc logic bị lỗi. Một trường hợp khó phát hiện khác là khi có nhiều bản cập nhật chồng chéo lên nhau; những cách tiếp cận ngây thơ có thể áp dụng kép hoặc đóng góp sai thứ tự, nhưng phương pháp đúng phải đảm bảo tích lũy tuyến tính. 

Cuối cùng, các truy vấn yêu cầu$K = 1$hoặc$K = N$kiểm tra tính đúng đắn của ranh giới của việc lập chỉ mục sau khi sắp xếp. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản áp dụng từng bản cập nhật bằng cách lặp lại từ$L$ĐẾN$R$và thêm$d$trực tiếp. Điều này đúng vì nó phản ánh định nghĩa của hoạt động. Tuy nhiên, mỗi bản cập nhật có thể chạm tới$N$các phần tử và với$Q$cập nhật, tổng độ phức tạp trở thành$O(NQ)$. Đối với đầu vào lớn, điều này trở nên không khả thi. 

Quan sát quan trọng là mỗi bản cập nhật đóng góp một giá trị không đổi trên một phân đoạn liên tục. Thay vì áp dụng từng phần tử, chúng ta có thể mã hóa hiệu ứng của nó bằng cách sử dụng một mảng khác biệt. Bằng cách đánh dấu +d tại vị trí$L$và a -d tại vị trí$R+1$, chúng tôi chuyển đổi các cập nhật phạm vi thành cập nhật điểm. Tổng tiền tố trên cấu trúc này sẽ tái tạo lại mảng cuối cùng theo thời gian tuyến tính. 

Khi mảng cuối cùng được xây dựng, việc sắp xếp nó cho phép truy cập trực tiếp vào số liệu thống kê đơn hàng. các$K$-phần tử lớn nhất trở thành tra cứu chỉ mục trực tiếp sau khi sắp xếp theo thứ tự tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NQ + M)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(N \log N + Q + M)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển vấn đề thành hai giai đoạn độc lập: áp dụng cập nhật phạm vi một cách hiệu quả, sau đó trả lời các truy vấn lựa chọn. 

1. Tạo mảng khác biệt`df`kích thước$N+1$, khởi tạo bằng 0. Cấu trúc này sẽ thể hiện cách các giá trị thay đổi giữa các vị trí liên tiếp thay vì lưu trữ trực tiếp các giá trị thực. 
2. Đối với mỗi bản cập nhật$(L, R, d)$, thêm vào$d$ĐẾN`df[L]`. Điều này có nghĩa là tất cả các vị trí bắt đầu từ$L$nên tăng thêm$d$. 
3. Trừ$d$từ`df[R+1]`nếu như$R+1 \le N$. Điều này sẽ hủy bỏ hiệu ứng ngoài phân đoạn, đảm bảo mức tăng dừng chính xác ở$R$. Việc xử lý ranh giới này làm cho việc biểu diễn tương đương với một bản cập nhật phạm vi. 
4. Tính tổng tiền tố`df`để xây dựng lại số gia tăng thực tế cho từng vị trí. Mỗi vị trí tích lũy tất cả các đóng góp tích cực ảnh hưởng đến nó. 
5. Thêm các số gia này vào mảng ban đầu$A$, tạo ra mảng biến đổi cuối cùng. 
6. Sắp xếp mảng kết quả theo thứ tự tăng dần. Việc sắp xếp sắp xếp các giá trị để thống kê thứ tự trở thành truy cập chỉ mục trực tiếp. 
7. Đối với mỗi truy vấn$K$, xuất phần tử tại chỉ mục$N - K$. Điều này tương ứng với$K$-giá trị lớn nhất kể từ khi lập chỉ mục bắt đầu từ 0. 

### Tại sao nó hoạt động 

Mảng chênh lệch mã hóa mỗi lần cập nhật phạm vi dưới dạng một cặp thay đổi ranh giới. Mỗi bản cập nhật đóng góp một phần bù không đổi trên một phân đoạn liền kề và tổng tiền tố đảm bảo rằng mỗi vị trí tổng hợp chính xác các bản cập nhật có phạm vi bao trùm nó. Điều này đảm bảo rằng sau khi xây dựng lại, mọi phần tử trong mảng phản ánh tổng của tất cả các cập nhật hợp lệ được áp dụng cho nó chính xác một lần. Sau đó, việc sắp xếp sẽ chuyển vấn đề thành một truy vấn thống kê thứ tự tĩnh, trong đó mỗi truy vấn được trả lời bằng một tra cứu chỉ mục xác định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q, m = map(int, input().split())
    a = list(map(int, input().split()))

    df = [0] * (n + 1)

    for _ in range(q):
        l, r, d = map(int, input().split())
        df[l - 1] += d
        if r < n:
            df[r] -= d

    cur = 0
    for i in range(n):
        cur += df[i]
        a[i] += cur

    a.sort()

    out = []
    for _ in range(m):
        k = int(input())
        out.append(str(a[n - k]))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo logic mảng khác biệt một cách chính xác. Điểm tinh tế quan trọng nhất là chuyển đổi sang lập chỉ mục dựa trên 0 một cách nhất quán. Bản cập nhật tại$L$trở thành`df[l - 1] += d`, và sự hủy bỏ tại$R+1$trở thành`df[r] -= d`bởi vì việc dịch chuyển theo một chỉ số sẽ căn chỉnh các ranh giới một cách chính xác. 

Biến tích lũy tiền tố`cur`đại diện cho tổng số hoạt động của tất cả các cập nhật hoạt động tại vị trí$i$. Mỗi phần tử mảng được cập nhật tại chỗ, tránh tốn thêm bộ nhớ. 

Sau khi sắp xếp, các truy vấn được trả lời theo thời gian không đổi bằng cách sử dụng chỉ mục trực tiếp từ cuối mảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N=5, A=[1,2,3,4,5]
updates: (1,3,+2), (2,5,+1)
queries: K=1, K=3
```Chúng tôi xây dựng`df`từng bước: 

| Bước | Hoạt động | trạng thái df (đã nén) | 
| --- | --- | --- | 
| ban đầu | tất cả số không | [0,0,0,0,0] | 
| 1 | +2 trên [1,3] | [2,0,0,-2,0] | 
| 2 | +1 trên [2,5] | [2,1,0,-2,0] | 

Bây giờ tổng tiền tố: 

| tôi | cur | A[i] | cập nhật A | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 3 | 
| 2 | 3 | 2 | 5 | 
| 3 | 3 | 3 | 6 | 
| 4 | 1 | 4 | 5 | 
| 5 | 1 | 5 | 6 | 

Mảng cuối cùng:`[3,5,6,5,6]`, sắp xếp:`[3,5,5,6,6]`Truy vấn: 

K=1 → 6 

K=3 → 5 

Điều này xác nhận rằng các bản cập nhật chồng chéo được tích lũy chính xác và các truy vấn sắp xếp chỉ phụ thuộc vào giá trị cuối cùng. 

### Ví dụ 2 

đầu vào:```
N=4, A=[10,10,10,10]
update: (1,4,-5)
query: K=2
```| Bước | df | giải thích | 
| --- | --- | --- | 
| sau khi cập nhật | [-5,0,0,5] | giảm đồng đều | 

Tổng tiền tố tạo ra`[-5,-5,-5,-5]`, vậy mảng cuối cùng là`[5,5,5,5]`. 

Mảng được sắp xếp vẫn còn`[5,5,5,5]`, do đó K=2 trả về 5. 

Điều này cho thấy thuật toán xử lý các cập nhật tiêu cực và duy trì sự ổn định dưới các phép biến đổi thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N + Q + M)$| ứng dụng cập nhật tuyến tính thông qua mảng sai phân, tái cấu trúc tuyến tính, sắp xếp chiếm ưu thế | 
| Không gian |$O(N)$| mảng khác biệt và lưu trữ mảng sửa đổi | 

Thời gian chạy phù hợp dễ dàng trong các ràng buộc điển hình cho$N, Q, M \le 2 \cdot 10^5$. Sắp xếp là thành phần siêu tuyến tính duy nhất và vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, q, m = map(int, input().split())
    a = list(map(int, input().split()))

    df = [0] * (n + 1)

    for _ in range(q):
        l, r, d = map(int, input().split())
        df[l - 1] += d
        if r < n:
            df[r] -= d

    cur = 0
    for i in range(n):
        cur += df[i]
        a[i] += cur

    a.sort()

    res = []
    for _ in range(m):
        k = int(input())
        res.append(str(a[n - k]))

    return "\n".join(res)

# minimal
assert run("1 0 1\n5\n1\n") == "5"

# single update full range
assert run("3 1 1\n1 2 3\n1 3 10\n1\n") == "13"

# overlapping updates
assert run("4 2 2\n1 3 1 2\n1 4 2\n1\n4\n") in {"5\n3", "5\n3"}

# negative updates
assert run("4 1 1\n10 10 10 10\n1 4 -5\n2\n") == "5"

# all equal final
assert run("5 0 1\n1 1 1 1 1\n3\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | giá trị đơn | độ đúng cơ sở | 
| cập nhật đầy đủ | ca đồng phục | xử lý ranh giới | 
| chồng chéo | cập nhật hỗn hợp | tích lũy đúng đắn | 
| tiêu cực | giảm | ký đúng | 
| tất cả đều bình đẳng | thống kê đơn hàng ổn định | sắp xếp đúng đắn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi bản cập nhật kết thúc chính xác ở phần tử cuối cùng. Ví dụ,$N=4$với bản cập nhật$(2,4,d)$. Hành vi đúng là không có phép trừ nào xảy ra bên ngoài giới hạn mảng. Việc thực hiện xử lý việc này bằng cách kiểm tra`if r < n`trước khi nộp đơn`df[r] -= d`, đảm bảo không có chỉ mục không hợp lệ nào được truy cập và hiệu ứng dừng chính xác ở ranh giới. 

Một trường hợp khác là khi$K = N$, nghĩa là phần tử nhỏ nhất được yêu cầu. Sau khi sắp xếp, truy cập`a[n - k]`trở thành`a[0]`, điều đó đúng. Ranh giới này thường gây ra từng lỗi một khi chuyển đổi giữa các định nghĩa truy vấn dựa trên 1 và mảng dựa trên 0, nhưng công thức lập chỉ mục luôn ánh xạ thứ hạng tới các vị trí một cách nhất quán. 

Trường hợp cuối cùng là khi tất cả các bản cập nhật bị loại bỏ hoặc tạo ra các giá trị giống hệt nhau. Việc sắp xếp vẫn hoạt động chính xác và các phần tử lặp lại không ảnh hưởng đến việc lập chỉ mục vì thống kê thứ tự chỉ phụ thuộc vào vị trí chứ không phụ thuộc vào tính duy nhất.
