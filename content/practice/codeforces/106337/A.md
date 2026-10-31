---
title: "CF 106337A - \u0418\u0442\u043e\u0433\u0438 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b"
description: "Chúng ta được cung cấp một mảng các số nguyên biểu thị điểm số của những người tham gia một cuộc thi Olympic. Đối với mỗi cặp người tham gia theo thứ tự $(i, j)$, chúng tôi tính toán xem $j$ có bao nhiêu điểm “thêm” so với $i$, nhưng chỉ khi $j$ tốt hơn."
date: "2026-06-19T08:54:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106337
codeforces_index: "A"
codeforces_contest_name: "2025-2026 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f, 1 \u0442\u0443\u0440"
rating: 0
weight: 106337
solve_time_s: 45
verified: true
draft: false
---

[CF 106337A - \u0418\u0442\u043e\u0433\u0438 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b](https://codeforces.com/problemset/problem/106337/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên biểu thị điểm số của những người tham gia một cuộc thi Olympic. Đối với mỗi cặp người tham gia được đặt hàng$(i, j)$, chúng tôi tính toán số điểm “thêm” là bao nhiêu$j$đã so sánh với$i$, nhưng chỉ khi$j$là tốt hơn. Chính thức, đối với mỗi cặp chúng tôi lấy$\max(a_j - a_i, 0)$, và chúng tôi tính tổng số này trên tất cả các cặp. 

Một cách khác để xem nhiệm vụ là ấn định người tham gia$i$và nhìn mọi người khác. Đối với mọi$j$với số điểm cao hơn$i$, người tham gia$i$đóng góp bằng không và người tham gia$j$đóng góp sự khác biệt$a_j - a_i$. Mục tiêu là tổng đóng góp tích lũy trên tất cả các so sánh như vậy. 

Kích thước đầu vào xác định mức độ tích cực của giải pháp. Một cách tiếp cận bậc hai kiểm tra trực tiếp tất cả các cặp trở nên không khả thi một khi$n$tăng lên hơn vài nghìn, vì$n^2$hoạt động đã vượt quá giới hạn thông thường đối với$n \approx 10^5$. Điều này ngay lập tức gợi ý rằng cấu trúc của biểu thức phải được đơn giản hóa để các đóng góp có thể được tổng hợp thay vì liệt kê từng cặp. 

Một trường hợp thất bại phổ biến đối với lý luận ngây thơ là xử lý sai thứ tự hoặc tính hai lần. Ví dụ, với đầu vào$[1, 3, 2]$, người ta có thể chỉ tính tổng không chính xác các hiệu liền kề hoặc quên rằng cả hai hướng so sánh đều được đưa vào nhưng không đối xứng do hàm max. Câu trả lời đúng yêu cầu xem xét tất cả các cặp có thứ tự, không chỉ các phần tử lân cận hoặc các phần tử lân cận được sắp xếp. 

Một cạm bẫy tinh vi khác là giả sử tính đối xứng: biểu thức có tính định hướng. Ví dụ,$a_j - a_i$chỉ đóng góp khi dương, do đó các chỉ số hoán đổi sẽ thay đổi liệu một cặp có đóng góp hay không. Bất kỳ cách tiếp cận nào coi các cặp là không có thứ tự sẽ thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu tuân theo định nghĩa trực tiếp. Đối với mỗi chỉ số$i$, chúng tôi lặp lại tất cả$j$, tính toán$a_j - a_i$, và thêm nó nếu nó dương. Điều này đúng vì nó kiểm tra rõ ràng mọi cặp có thứ tự và áp dụng công thức chính xác như được xác định. Tuy nhiên, điều này đòi hỏi$n^2$đánh giá và mỗi lần đánh giá đều có thời gian không đổi, do đó tổng công việc tăng theo phương trình bậc hai. Đối với đầu vào lớn, điều này trở nên quá chậm. 

Quan sát quan trọng là biểu thức chỉ phụ thuộc vào thứ tự tương đối của các giá trị chứ không phụ thuộc vào vị trí của chúng trong mảng ban đầu. Đối với một cặp cố định$i, j$, chỉ có các giá trị quan trọng: nếu$a_i \ge a_j$, phần đóng góp bằng không; nếu không thì nó là$a_j - a_i$. Điều này cho phép chúng ta tổ chức lại việc tính toán bằng cách nhóm các giá trị bằng nhau hoặc có thứ tự lại với nhau. 

Sau khi các giá trị được sắp xếp, mỗi cặp$i < j$đóng góp chính xác$a_j - a_i$, bởi vì đảm bảo đặt hàng$a_j \ge a_i$. Điều này loại bỏ hoàn toàn sự cần thiết của hàm max, vì số âm không bao giờ xuất hiện theo thứ tự được sắp xếp. Vấn đề giảm xuống còn tổng các khác biệt trên tất cả các cặp tăng dần, có thể được biểu thị bằng tổng tiền tố: cho mỗi vị trí$i$, chúng tôi tính toán lớn hơn bao nhiêu$a_i$hơn tất cả các yếu tố trước đó và tích lũy những đóng góp đó một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Sắp xếp + tổng tiền tố | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Điều này đảm bảo rằng với bất kỳ cặp nào$i < j$, chúng tôi có$a_i \le a_j$, giúp loại bỏ sự cần thiết phải kiểm tra có điều kiện bên trong tổng. 
2. Duy trì tổng tiền tố đang chạy của các phần tử được xử lý. Điều này cho phép chúng tôi tính toán tổng đóng góp của các phần tử trước đó trong thời gian không đổi trên mỗi vị trí. 
3. Lặp lại mảng đã được sắp xếp. Tại mỗi chỉ số$i$, đối xử$a[i]$là phần tử lớn hơn trong tất cả các cặp$(j, i)$Ở đâu$j < i$. 
4. Tính toán sự đóng góp của$a[i]$BẰNG$i \cdot a[i] - \text{prefixSum}$. Biểu thức này thể hiện tổng sự khác biệt giữa$a[i]$và tất cả các phần tử trước đó, được mở rộng theo đại số từ$\sum (a[i] - a[j])$. 
5. Thêm phần đóng góp này vào câu trả lời cuối cùng. 
6. Cập nhật tổng tiền tố bằng cách thêm$a[i]$, chuẩn bị cho lần lặp tiếp theo. 

Ý tưởng chính là mỗi phần tử được xử lý dưới dạng “điểm cuối bên phải” của tất cả các cặp liên quan đến các phần tử nhỏ hơn và tất cả những đóng góp như vậy được tổng hợp trong một biểu thức số học. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mọi đóng góp hợp lệ$\max(a_j - a_i, 0)$trở nên đơn giản$a_j - a_i$khi$j > i$, vì việc đặt hàng đảm bảo tính không âm. Mỗi phần tử$a[i]$đóng góp chính xác một lần làm cạnh lớn hơn của tất cả các cặp có phần tử trước đó và tổng tiền tố đảm bảo chúng ta trừ tổng các phần tử nhỏ hơn đó một cách chính xác. Điều này đảm bảo rằng mỗi cặp đặt hàng được tính chính xác một lần, với sự khác biệt được ký chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort()
    
    res = 0
    pref = 0
    
    for i, x in enumerate(a):
        res += i * x - pref
        pref += x
    
    print(res)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng sao cho việc so sánh trở nên có tính định hướng và đơn điệu. Biến`pref`lưu trữ tổng của tất cả các phần tử trước chỉ mục hiện tại. Ở mỗi bước,`i * x`biểu thị tổng đóng góp nếu tất cả các phần tử trước đó bằng 0 và trừ đi`pref`sửa lỗi này để tính đến giá trị thực tế của chúng, tạo ra tổng chênh lệch. 

Một lỗi phổ biến là cập nhật`pref`trước khi tính toán phần đóng góp, điều này sẽ đưa phần tử hiện tại vào phép tính của chính nó không chính xác. Thứ tự hiển thị ở đây tránh được vấn đề đó bằng cách trước tiên tính toán phần đóng góp, sau đó cập nhật tổng tiền tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 3 2
```Mảng được sắp xếp:`[1, 2, 3]`| tôi | x | trước | đóng góp = i*x - pref | trước sau | độ phân giải | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 1 | 0 | 
| 1 | 2 | 1 | 1 | 3 | 1 | 
| 2 | 3 | 3 | 3 | 6 | 4 | 

Kết quả cuối cùng là 4. 

Dấu vết này cho thấy rằng mỗi phần tử chỉ đóng góp dựa trên các phần tử trước đó, xác nhận rằng mỗi cặp được tính chính xác một lần dưới dạng chênh lệch chuyển tiếp. 

### Ví dụ 2 

đầu vào:```
4
5 5 5 5
```Mảng được sắp xếp vẫn còn`[5, 5, 5, 5]`| tôi | x | trước | đóng góp | trước sau | độ phân giải | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 0 | 0 | 5 | 0 | 
| 1 | 5 | 5 | 0 | 10 | 0 | 
| 2 | 5 | 10 | 0 | 15 | 0 | 
| 3 | 5 | 15 | 0 | 20 | 0 | 

Điều này xác nhận rằng các phần tử bằng nhau không tạo ra sự đóng góp nào, vì không tồn tại sự khác biệt tích cực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét tuyến tính đơn theo sau | 
| Không gian | O(1) | Ngoài bộ nhớ đầu vào, chỉ có một số biến được sử dụng | 

Độ phức tạp nằm trong giới hạn điển hình cho$n$lên đến$10^5$, trong đó việc sắp xếp và một lần chuyển là đủ hiệu quả trong giới hạn tiêu chuẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# basic sample-style case
assert run("3\n1 3 2\n") == "4"

# all equal
assert run("5\n7 7 7 7 7\n") == "0"

# strictly increasing
assert run("4\n1 2 3 4\n") == "10"

# strictly decreasing
assert run("4\n4 3 2 1\n") == "10"

# single element
assert run("1\n42\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 3 2 | 4 | thứ tự hỗn hợp cơ bản | 
| 5 7 7 7 7 7 | 0 | giá trị bằng nhau | 
| 4 1 2 3 4 | 10 | cơ cấu tăng dần | 
| 4 4 3 2 1 | 10 | giảm tính đối xứng đầu vào | 
| 1 42 | 0 | trường hợp cạnh tối thiểu | 

## Vỏ cạnh 

Đối với các giá trị bằng nhau, mảng được sắp xếp không tạo ra sự đóng góp nào ở mỗi bước vì`i * x`khớp chính xác với tổng tiền tố được chia tỷ lệ trên các phần tử giống hệt nhau. Đối với đầu vào`5 7 7 7 7 7`, mỗi lần lặp mang lại sự khác biệt bằng 0 vì không có phần tử nào hoàn toàn nhỏ hơn hoặc lớn hơn theo cách tạo ra sự khác biệt tích cực. 

Đối với đầu vào giảm nghiêm ngặt như`4 3 2 1`, sắp xếp đảo ngược nó thành`1 2 3 4`và thuật toán tính toán tất cả các khác biệt theo cặp một lần theo đúng hướng. Tại`i = 3`, giá trị`4`đóng góp`3*4 - (1+2+3) = 12 - 6 = 6`, tương ứng với sự khác biệt với tất cả các phần tử trước đó. Tương tự, mỗi bước trước đó sẽ tích lũy tất cả các cặp hợp lệ, đảm bảo phạm vi bao phủ đầy đủ mà không bị trùng lặp.
