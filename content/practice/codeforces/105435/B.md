---
title: "CF 105435B - Truy vấn số chia"
description: "Chúng ta được cung cấp một chuỗi các truy vấn, mỗi truy vấn mô tả một khoảng số $[l, r]$. Đối với mỗi truy vấn, chúng tôi được yêu cầu đếm xem có bao nhiêu số nguyên trong khoảng đó có thuộc tính cụ thể liên quan đến số ước của chúng."
date: "2026-06-23T03:48:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105435
codeforces_index: "B"
codeforces_contest_name: "TSEC Round 2 (Div. 3)"
rating: 0
weight: 105435
solve_time_s: 82
verified: true
draft: false
---

[CF 105435B - Truy vấn số chia](https://codeforces.com/problemset/problem/105435/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các truy vấn, mỗi truy vấn mô tả một khoảng số$[l, r]$. Đối với mỗi truy vấn, chúng tôi được yêu cầu đếm xem có bao nhiêu số nguyên trong khoảng đó có thuộc tính cụ thể liên quan đến số ước của chúng. Một loại truy vấn yêu cầu các số nguyên có số ước là số lẻ và loại truy vấn kia yêu cầu các số nguyên có số ước là số chẵn. 

Một quan sát quan trọng đến từ lý thuyết số hơn là mô phỏng. Hầu hết mọi số nguyên đều có các ước số đi đôi với nhau: nếu$d$chia rẽ$n$, sau đó$n/d$cũng chia$n$. Việc ghép đôi này cho thấy số lượng ước số thường là số chẵn. Trường hợp duy nhất một số chia không ghép đôi với một đối tác riêng biệt là khi$d = n/d$, nghĩa$d^2 = n$. Điều đó xảy ra chính xác khi$n$là một hình vuông hoàn hảo 

Điều này ngay lập tức làm thay đổi vấn đề: các số có số ước số lẻ chính xác là những số chính phương hoàn hảo, và tất cả các số khác có số ước số chẵn. Vì vậy, mỗi truy vấn thực sự là hỏi một trong hai điều trong một phạm vi: đếm các ô vuông hoàn hảo hoặc đếm các ô vuông không hoàn hảo. 

Các ràng buộc đẩy chúng tôi ra khỏi mọi quá trình xử lý theo số trong mỗi truy vấn. Với tối đa$10^6$truy vấn và giá trị lên đến$10^9$, việc lặp lại các phạm vi hoặc kiểm tra ước số cho mỗi số là không thể. Ngay cả công việc logarit trên mỗi truy vấn cũng sẽ ở mức giới hạn, do đó, giải pháp phải giảm từng truy vấn về số học theo thời gian không đổi sau một số lý luận hoặc tính toán trước. 

Các trường hợp cạnh phát sinh khi$l = r$, trong đó khoảng chứa một số duy nhất và khi khoảng này vượt qua ranh giới của các hình vuông hoàn hảo. Ví dụ, trong$[8, 9]$, chỉ có 9 là số chính phương nên có đúng một số có ước số lẻ. Một cách tiếp cận đơn giản để kiểm tra trực tiếp các ước số vẫn có hiệu quả về mặt khái niệm nhưng sẽ hết thời gian. Một vấn đề tế nhị khác là hiểu sai quy tắc chẵn lẻ: việc kiểm tra các ước số trực tiếp trên mỗi số sẽ thất bại trong điều kiện hạn chế về thời gian mặc dù về mặt logic vẫn đúng. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng truy vấn một cách độc lập. Với mọi số nguyên trong$[l, r]$, chúng ta tính số ước của nó bằng cách lặp đến căn bậc hai của nó và đếm các cặp thừa số. Điều này xác định chính xác số lượng ước số là số lẻ hay số chẵn. Tuy nhiên, kích thước khoảng có thể lớn và lên tới$10^6$truy vấn, điều này trở nên không thể tính toán được. Trong trường hợp xấu nhất, ngay cả một truy vấn đơn lẻ trên phạm vi rộng cũng sẽ tốn kém$O(\sqrt{n})$mỗi số, dẫn đến khoảng$O((r-l+1)\sqrt{r})$, vượt xa giới hạn khả thi. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các ước số một cách rõ ràng và thay vào đó hãy phân loại các số theo cấu trúc. Vì chỉ có các ô vuông hoàn hảo mới có ước số không ghép đôi nên toàn bộ vấn đề sẽ giảm xuống việc đếm các ô vuông hoàn hảo bên trong một phạm vi. Khi điều này được nhận ra, mỗi truy vấn sẽ trở thành một bài toán số học đơn giản: đếm xem có bao nhiêu số nguyên$x$thỏa mãn$l \le x \le r$Và$x = k^2$. Điều đó tương đương với việc đếm số nguyên$k$như vậy$\lceil \sqrt{l} \rceil \le k \le \lfloor \sqrt{r} \rfloor$. Điều này làm giảm mọi truy vấn về thời gian không đổi bằng cách sử dụng các phép tính căn bậc hai số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((r-l+1)\sqrt{r})$mỗi truy vấn |$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng các số có ước số lẻ chính xác là số chính phương. Điều này thay thế việc tính toán số chia bằng việc kiểm tra thuộc tính cấu trúc. 
2. Đối với mỗi truy vấn$(type, l, r)$, tính giới hạn căn bậc hai số nguyên của khoảng. Nhỏ nhất$k$như vậy$k^2 \ge l$là$\lceil \sqrt{l} \rceil$, và lớn nhất$k$như vậy$k^2 \le r$là$\lfloor \sqrt{r} \rfloor$. 
3. Khi đó số bình phương hoàn hảo trong khoảng đó là$\max(0, \lfloor \sqrt{r} \rfloor - \lceil \sqrt{l} \rceil + 1)$. Điều này trực tiếp trả lời các truy vấn loại 1. 
4. Đối với truy vấn loại 2, kích thước khoảng là$r - l + 1$và tất cả các số không phải là số chính phương đều được tính bằng cách trừ đi số bình phương. 

Mỗi bước chuyển đổi một bài toán đếm tổ hợp thành số học trên các chỉ số của bình phương thay vì trên các giá trị ban đầu. 

### Tại sao nó hoạt động 

Mọi số nguyên dương đều có cấu trúc ghép chia số$d \leftrightarrow n/d$. Những cặp này khác biệt trừ khi$d = n/d$, điều đó chỉ xảy ra khi$n$là một hình vuông hoàn hảo Ước số không ghép đôi đó làm cho tổng số lẻ. Vì có chính xác một ước số không ghép đôi như vậy cho mỗi số bình phương và không có ước nào khác, nên việc đếm các số chia lẻ tương đương với việc đếm các bình phương. Thuật toán dựa hoàn toàn vào sự kết hợp giữa “số ước lẻ” và “số bình phương trong khoảng”, đảm bảo tính chính xác cho tất cả đầu vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    q = int(input())
    out = []
    
    for _ in range(q):
        t, l, r = map(int, input().split())
        
        left = math.isqrt(l)
        if left * left < l:
            left += 1
        right = math.isqrt(r)
        
        squares = 0
        if right >= left:
            squares = right - left + 1
        
        total = r - l + 1
        
        if t == 1:
            out.append(str(squares))
        else:
            out.append(str(total - squares))
    
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp phụ thuộc vào`math.isqrt`, cung cấp căn bậc hai số nguyên chính xác mà không có lỗi dấu phẩy động. Việc điều chỉnh ranh giới bên trái đảm bảo chúng tôi xử lý chính xác các trường hợp$l$bản thân nó là một hình vuông hoàn hảo; nếu không chúng ta sẽ dịch chuyển phạm vi không chính xác. 

Tổng số được tính cho mỗi truy vấn và số bình phương được lấy hoàn toàn từ số học số nguyên. Không cần tính toán trước vì mỗi truy vấn là độc lập. 

## Ví dụ đã hoạt động 

Hãy xem xét một phạm vi trong đó chúng tôi truy vấn cả hai loại: 

đầu vào:```
1
1 8 20
```Chúng tôi tính toán giới hạn bình phương số nguyên. 

| Bước | tôi | r | sàn(sqrt(r)) | trần(sqrt(l)) | hình vuông | kết quả loại 1 | kết quả loại 2 | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 8 | 20 | - | - | - | - | - | 
| giới hạn sqrt | 8 | 20 | 4 | 3 | 4-3+1=2 | 2 | 11 | 

Các số bình phương hoàn hảo là 9 và 16, vì vậy loại 1 trả về 2 và loại 2 trả về 9 số còn lại. 

Bây giờ hãy xem xét một trường hợp nặng về ranh giới: 

đầu vào:```
1
1 1 10
```| Bước | tôi | r | sàn(sqrt(r)) | trần(sqrt(l)) | hình vuông | loại 1 | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 10 | - | - | - | - | 
| giới hạn sqrt | 1 | 10 | 3 | 1 | 3 | 3 | 

Các ô vuông là 1, 4, 9, cho đúng 3 số hợp lệ. Điều này xác nhận tính đúng đắn khi cả hai ranh giới đều là hình vuông hoàn hảo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q)$| Mỗi truy vấn thực hiện các phép tính căn bậc hai số nguyên theo thời gian không đổi và số học | 
| Không gian |$O(1)$| Chỉ sử dụng một số biến cố định | 

Giải pháp xử lý thoải mái$10^6$các truy vấn vì mỗi truy vấn được rút gọn thành một số phép toán số nguyên. Ngay cả trong Python, căn bậc hai số nguyên cũng đủ nhanh cho thang đo này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def solve():
        q = int(input())
        out = []
        for _ in range(q):
            t, l, r = map(int, input().split())
            left = math.isqrt(l)
            if left * left < l:
                left += 1
            right = math.isqrt(r)
            squares = max(0, right - left + 1)
            total = r - l + 1
            if t == 1:
                out.append(str(squares))
            else:
                out.append(str(total - squares))
        return "\n".join(out)

    return solve()

# provided sample (partial format assumed)
assert run("""1
1 1 10
""") == "3"

# all equal bounds
assert run("""1
1 16 16
""") == "1"

# non-square single element
assert run("""1
1 8 8
""") == "0"

# full range mix
assert run("""1
2 1 10
""") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 10 (loại 1) | 3 | đếm bình phương đúng trong phạm vi | 
| 16 16 | 1 | trường hợp ranh giới hình vuông hoàn hảo duy nhất | 
| 8 8 | 0 | trường hợp không vuông đơn | 
| 2 1 10 | 7 | logic bổ sung đúng đắn | 

## Vỏ cạnh 

Một trường hợp cạnh tranh quan trọng là khi$l$chính nó là một hình vuông hoàn hảo. Ví dụ,$[9, 9]$. Căn bậc hai nguyên của 9 là 3, và vì$3^2 = 9$, giới hạn dưới phải bao gồm 3. Bước điều chỉnh trong mã đảm bảo chúng tôi không loại trừ nhầm. 

Một trường hợp khác là khi không có ô vuông nào trong khoảng, chẳng hạn như$[8, 8]$. Đây$\lfloor \sqrt{8} \rfloor = 2$Và$\lceil \sqrt{8} \rceil = 3$, tạo ra số đếm âm. Việc triển khai bảo vệ chống lại điều này bằng cách ngầm định ở mức 0 thông qua`if right >= left`kiểm tra. 

Khoảng cách lớn như$[1, 10^9]$nhấn mạnh tính đúng đắn của ranh giới căn bậc hai số nguyên. Thuật toán vẫn quy giản bài toán xuống việc đếm các số nguyên từ 1 đến 31622, cho thấy phép biến đổi vẫn ổn định ngay cả ở quy mô lớn.
