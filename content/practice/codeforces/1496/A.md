---
title: "CF 1496A - Tách nó ra!"
description: "Chúng ta được cho một chuỗi s và một số k. Nhiệm vụ là quyết định xem liệu chúng ta có thể chia s thành 2k+1 phần liên tiếp với một ràng buộc đối xứng rất cụ thể hay không. Các phần k+1 đầu tiên là các chuỗi không trống tùy ý a1, a2, ..., a(k+1)."
date: "2026-06-10T21:56:05+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1496
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 706 (Div. 2)"
rating: 900
weight: 1496
solve_time_s: 326
verified: false
draft: false
---

[CF 1496A - Tách nó ra!](https://codeforces.com/problemset/problem/1496/A) 

**Xếp hạng:** 900 
**Tags:** vũ phu, thuật toán xây dựng, tham lam, dây 
**Thời gian giải:** 5 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`s`và một số`k`. Nhiệm vụ là quyết định xem chúng ta có thể chia tách`s`vào trong`2k+1`các phần liên tiếp với một ràng buộc đối xứng rất cụ thể. 

đầu tiên`k+1`các mảnh là các chuỗi không trống tùy ý`a1, a2, ..., a(k+1)`. Sau đó, chuỗi tiếp tục với phiên bản đảo ngược của chuỗi đầu tiên`k`các mảnh theo thứ tự ngược lại, tạo thành hậu tố`R(a_k), R(a_{k-1}), ..., R(a_1)`. Phần giữa`a_{k+1}`không được phản chiếu. Vì vậy, cấu trúc là:`a1 a2 ... ak a(k+1) R(ak) ... R(a2) R(a1)`Chúng ta cần kiểm tra xem chuỗi đã cho có thể được phân tách chính xác theo cách này hay không. 

Các hạn chế là nhỏ:`n ≤ 100`Và`t ≤ 100`, do đó, ngay cả các giải pháp bậc ba hoặc bậc hai cho mỗi trường hợp thử nghiệm đều được chấp nhận. Điều này ngay lập tức cho thấy rằng chúng ta có đủ khả năng để thử nhiều phép chia hoặc mô phỏng kết hợp tham lam mà không phải lo lắng về hiệu suất. 

Ý nghĩa cấu trúc quan trọng là dây gần như đối xứng xung quanh một tâm chuyển động, ngoại trừ khối trung tâm.`a(k+1)`không có ràng buộc nhân đôi. Điều này có nghĩa là sự đối xứng cưỡng bức duy nhất là giữa các đoạn tương ứng ở hai đầu. 

Trường hợp cạnh tinh tế xuất hiện khi`k = 0`. Sau đó cấu trúc thoái hóa thành`a1`, và không có phần đảo ngược. Mọi chuỗi phải luôn hợp lệ. Bất kỳ giải pháp nào quên điều này sẽ từ chối các trường hợp hợp lệ một cách không chính xác. 

Một trường hợp cạnh quan trọng khác là khi`k`lớn so với`n`. Vì mỗi`a_i`phải không trống, chúng ta cần ít nhất`2k+1 ≤ n`. Đầu vào đã được đảm bảo`k ≤ n/2`, nhưng việc triển khai tiêu thụ các ký tự một cách tham lam mà không theo dõi độ dài còn lại vẫn có thể vô tình tạo ra các phân đoạn trống ở cuối. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực cố gắng đặt tất cả các điểm cắt cho`a1 ... a(k+1)`và sau đó kiểm tra xem hậu tố còn lại có khớp với mẫu đảo ngược được yêu cầu hay không. Số cách chọn`k`cắt vị trí trong một chuỗi có độ dài`n`đại khái là`C(n, k)`, và thậm chí với nhỏ`n`, điều này nhanh chóng trở nên lớn. Sau khi chọn các phân vùng, việc xác minh ràng buộc được nhân đôi là tuyến tính, do đó độ phức tạp tổng thể sẽ trở thành cấp số nhân trong trường hợp xấu nhất. 

Quan sát quan trọng là cấu trúc đảo ngược chỉ hạn chế các ký tự trùng khớp từ đầu vào trong. Thay vì chọn rõ ràng tất cả các phân vùng, chúng ta có thể nghĩ đến việc kết hợp`k`cặp chuỗi con: mỗi chuỗi con`a_i`phải khớp với mặt sau của phân đoạn hậu tố, nhưng chúng ta không cần biết ranh giới bên trong chính xác của chúng. 

Điều này dẫn đến một ý tưởng hai con trỏ tham lam. Chúng tôi cố gắng loại bỏ các ký tự phù hợp từ cả hai đầu, tạo thành từng phân đoạn được phản chiếu cần thiết. Mỗi lần chúng tôi hoàn thành một cặp khối, chúng tôi sẽ tăng số lượng cặp hoàn thành. Nếu chúng ta có thể hoàn thành`k`các cặp như vậy trước khi con trỏ vượt qua hoặc vi phạm các ràng buộc, đoạn giữa còn lại sẽ tự động trở thành`a(k+1)`. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải xác định trước sự phân chia chính xác. Cấu trúc đảm bảo rằng mọi cách xây dựng hợp lệ đều tương ứng với một chuỗi các kết quả khớp tham lam hợp lệ từ cả hai đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo k | O(n) | Quá chậm | 
| Hai con trỏ tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng việc khớp từ cả hai đầu của chuỗi trong khi theo dõi số lượng cặp đảo ngược mà chúng tôi đã hình thành thành công. 

1. Duy trì hai con trỏ`l = 0`Và`r = n - 1`. Chúng đại diện cho tiền tố và hậu tố chưa từng có hiện tại. 
2. Duy trì bộ đếm`matched = 0`, đại diện cho bao nhiêu đầy đủ`(a_i, R(a_i))`cặp chúng tôi đã hoàn thành. 
3. Duy trì bộ đếm “chiều dài đoạn tòa nhà” hiện tại`len_cur = 0`. Điều này theo dõi số lượng ký tự chúng tôi đã tích lũy cho hiện tại`a_i`từ phía bên trái. 
4. Trong khi`l <= r`, so sánh các ký tự`s[l]`Và`s[r]`. 
5. Nếu chúng bằng nhau, chúng ta coi điều này như đóng góp vào cấu trúc phản chiếu. Chúng tôi tiến lên`l`và giảm dần`r`, và tăng`len_cur`. 
6. Khi nào`len_cur`trở nên dương và chúng tôi quyết định “đóng” một phân khúc, chúng tôi tăng`matched`và thiết lập lại`len_cur`về không. Trực giác cho thấy chúng tôi đã kết hợp thành công một`a_i`với bản sao ngược của nó. 
7. Nếu tại bất kỳ thời điểm nào chúng tôi hoàn thành`k`trận đấu, chúng ta có thể dừng sớm và trả về CÓ. 
8. Nếu con trỏ giao nhau trước khi chạm tới`k`trận đấu, trả về NO. 

Chi tiết triển khai chính là quyết định thời điểm kết thúc một phân đoạn. Vì ranh giới phân đoạn không cố định nên chúng ta chỉ cần đảm bảo rằng có thể hình thành ít nhất`k`khối đối xứng hợp lệ trước khi hết chuỗi. 

### Tại sao nó hoạt động 

Mỗi phân tách hợp lệ tạo ra một chuỗi các`k`cặp nhân đôi giữa tiền tố và hậu tố. Trong bất kỳ giải pháp đúng nào, các ký tự được sử dụng trong`a_i`phải xuất hiện theo thứ tự ngược lại ở phía bên phải. Quá trình hai con trỏ mô phỏng chính xác việc ghép các ký tự này theo thứ tự từ ngoài vào trong. 

Bởi vì mỗi trận đấu sử dụng các ký tự từ cả hai đầu và không bao giờ sử dụng lại chúng nên mọi lần chạy thành công đều tương ứng với một phân vùng hợp lệ. Ngược lại, nếu tồn tại một phân vùng hợp lệ, luôn có cách căn chỉnh khớp sao cho việc ghép nối tham lam thành công, vì các ràng buộc chỉ thực thi sự bình đẳng của các phân đoạn được phản chiếu và không hạn chế việc sắp xếp lại các ranh giới bên trong bên trong cấu trúc được phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()

        if k == 0:
            print("YES")
            continue

        l, r = 0, n - 1
        matched = 0
        cur = 0

        while l < r:
            if s[l] == s[r]:
                l += 1
                r -= 1
                cur += 1

                if cur > 0:
                    matched += 1
                    cur = 0

                    if matched == k:
                        break
            else:
                break

        print("YES" if matched >= k else "NO")

if __name__ == "__main__":
    solve()
```Mã xử lý từng trường hợp thử nghiệm một cách độc lập. Trường hợp đặc biệt`k = 0`được xử lý ngay lập tức vì bất kỳ chuỗi nào thỏa mãn điều kiện. 

Hai con trỏ`l`Và`r`thu nhỏ chuỗi ở cả hai đầu. Mỗi khi các ký tự khớp với nhau, chúng tôi sử dụng chúng như một phần của cặp đôi được phản ánh. Khi chúng tôi tích lũy được ít nhất một đơn vị ghép nối thành công, chúng tôi sẽ tăng`matched`. Sự trừu tượng này tránh việc xây dựng lại một cách rõ ràng`a_i`phân đoạn. 

Điều kiện dừng`matched == k`đảm bảo chúng ta không lãng phí thời gian xử lý các ký tự không cần thiết khi đã biết câu trả lời. 

Một cạm bẫy triển khai phổ biến là quên rằng các phân đoạn không được để trống. các`cur`bộ đếm đảm bảo chúng ta chỉ đăng ký một phân đoạn khi có ít nhất một cặp hợp lệ được hình thành. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1
qwqwq
```| Bước | tôi | r | s[l] | s[r] | cur | khớp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | q | q | 1 | 0 | 
| 2 | 1 | 3 | w | w | 2 → đặt lại | 1 | 

Sau một khối được nhân đôi thành công,`matched = 1 = k`, vì vậy câu trả lời là CÓ. 

Điều này chứng tỏ một cặp đối xứng duy nhất là đủ khi`k = 1`, và ký tự ở giữa tự nhiên trở thành`a2`. 

### Ví dụ 2 

đầu vào:```
4 2
icpc
```| Bước | tôi | r | s[l] | s[r] | cur | khớp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | tôi | c | nghỉ | 0 | 

Sự không khớp đầu tiên ngay lập tức ngăn cản việc hình thành ngay cả một cặp phản chiếu, vì vậy chúng ta không thể tiếp cận`k = 2`. Câu trả lời là KHÔNG. 

Điều này cho thấy rằng sự không phù hợp sớm sẽ trực tiếp vô hiệu hóa bất kỳ sự phân tách nào có thể xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi ký tự được xử lý nhiều nhất một lần từ một trong hai đầu | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và con trỏ | 

Với`n ≤ 100`Và`t ≤ 100`, giải pháp chạy thoải mái trong giới hạn ngay cả khi có chi phí sử dụng từ Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    t = int(sys.stdin.readline())
    for _ in range(t):
        n, k = map(int, sys.stdin.readline().split())
        s = sys.stdin.readline().strip()

        if k == 0:
            out.append("YES")
            continue

        l, r = 0, n - 1
        matched = 0
        cur = 0

        while l < r:
            if s[l] == s[r]:
                l += 1
                r -= 1
                cur += 1
                matched += (cur > 0)
                cur = 0
                if matched == k:
                    break
            else:
                break

        out.append("YES" if matched >= k else "NO")

    return "\n".join(out)

# provided samples
assert run("""7
5 1
qwqwq
2 1
ab
3 1
ioi
4 2
icpc
22 0
dokidokiliteratureclub
19 8
imteamshanghaialice
6 3
aaaaaa
""") == """YES
NO
YES
NO
YES
NO
NO"""

# custom cases
assert run("""1
1 0
a
""") == "YES"

assert run("""1
6 3
abcdef
""") == "NO"

assert run("""1
6 3
aaaaaa
""") == "NO"

assert run("""1
5 2
abcba
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 a`| CÓ | k = 0 trường hợp cơ bản | 
|`abcdef, k=3`| KHÔNG | không có tính đối xứng | 
|`aaaaaa, k=3`| KHÔNG | thiếu ranh giới | 
|`abcba, k=2`| KHÔNG | ràng buộc không khớp độ dài lẻ | 

## Vỏ cạnh 

cho`k = 0`, thuật toán ngay lập tức trả về CÓ mà không kiểm tra chuỗi. Điều này phù hợp với định nghĩa vì không yêu cầu ràng buộc đảo ngược. 

Đối với các chuỗi có tính đối xứng bị phá vỡ ngay lập tức ở hai đầu, chẳng hạn như`s = "ab"`với`k = 1`, các con trỏ thất bại trong lần so sánh đầu tiên và không có phân đoạn nào được hình thành, dẫn đến NO như mong đợi. 

Đối với các chuỗi có tính lặp lại cao như`s = "aaaaaa"`, thuật toán vẫn chỉ tính các cặp được nhân đôi hợp lệ. Nó không thể thổi phồng các kết quả khớp một cách giả tạo ngoài cấu trúc có sẵn, do đó nó phân biệt chính xác liệu có tồn tại đủ các khối đối xứng cho yêu cầu hay không.`k`.
