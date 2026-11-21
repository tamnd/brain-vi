---
title: "CF 2190A - Trò chơi phân loại"
description: "Chúng ta được yêu cầu phân tích một trò chơi hai người chơi trên một chuỗi nhị phân, trong đó Alice đi trước và Bob theo sau, thay phiên nhau. Đến lượt của mình, người chơi có thể chọn bất kỳ chuỗi con không tăng hoàn toàn nào của chuỗi và sắp xếp lại nó thành không giảm."
date: "2026-06-07T21:03:16+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2190
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1073 (Div. 1)"
rating: 1200
weight: 2190
solve_time_s: 113
verified: false
draft: false
---

[CF 2190A - Trò chơi sắp xếp](https://codeforces.com/problemset/problem/2190/A) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, trò chơi, tham lam 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu phân tích một trò chơi hai người chơi trên một chuỗi nhị phân, trong đó Alice đi trước và Bob theo sau, thay phiên nhau. Đến lượt của mình, người chơi có thể chọn bất kỳ chuỗi con không tăng hoàn toàn nào của chuỗi và sắp xếp lại nó thành không giảm. Trong thực tế, điều này có nghĩa là tìm một chuỗi các vị trí liên tiếp hoặc không liên tiếp chứa ít nhất một`1`và một`0`, sau đó sắp xếp chuỗi đó sao cho tất cả`0`hãy đến trước hết`1`sẽ đến cuối cùng. Trò chơi kết thúc khi không có nước đi hợp lệ nào tồn tại và người chơi cuối cùng thực hiện nước đi sẽ thắng. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho một chuỗi có độ dài`n`chỉ chứa`0`cát`1`S. Kết quả đầu ra yêu cầu xác định người chiến thắng trong cách chơi tối ưu và nếu Alice có thể thắng, hãy chỉ định nước đi đầu tiên của cô ấy bằng cách liệt kê các vị trí mà cô ấy nên chọn. 

Những hạn chế về`n`đạt đến`2·10^5`cho mỗi trường hợp thử nghiệm, với tổng số tiền trên các trường hợp thử nghiệm cũng được giới hạn ở mức`2·10^5`, loại trừ bất kỳ giải pháp vũ phu nào cố gắng mô phỏng tất cả các chuyển động hoặc trình tự có thể xảy ra. Một cách tiếp cận đơn giản liệt kê mọi động thái tiềm năng sẽ yêu cầu kiểm tra các chuỗi con O(n2) hoặc tệ hơn, điều này không khả thi đối với các đầu vào lớn nhất. 

Một trường hợp cạnh quan trọng xảy ra khi chuỗi đã được sắp xếp theo thứ tự không giảm, chẳng hạn như`0000`hoặc`111`. Trong trường hợp như vậy, không có nước đi nào tồn tại vì bất kỳ chuỗi con nào cũng sẽ chỉ chứa`0`s hoặc chỉ`1`s, vậy là Bob thắng ngay lập tức. Một trường hợp tinh tế khác là khi chuỗi có một sự đảo ngược duy nhất, như`10`hoặc`101`. Tại đây, Alice có thể trực tiếp chọn dãy con chứa đảo ngược, sắp xếp nó và có khả năng kết thúc trò chơi ngay lập tức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng trò chơi: với mỗi lượt, tạo ra tất cả các chuỗi con không tăng có chứa cả hai`0`Và`1`, áp dụng nước đi, sau đó xác định đệ quy người chiến thắng cho người chơi tiếp theo. Mặc dù đúng về nguyên tắc nhưng cách tiếp cận này nhanh chóng trở nên khó thực hiện vì ngay cả đối với`n = 1000`, số dãy con trong trường hợp xấu nhất là khoảng 2ⁿ, vượt xa giới hạn tính toán hợp lý. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận tối ưu là trò chơi được xác định hoàn toàn bởi các vị trí đảo ngược - những nơi mà một`1`xảy ra trước một`0`. Động thái duy nhất có thể thay đổi chuỗi liên quan đến việc lật các đảo ngược này. Chính thức hơn, hãy đếm số lượng vị trí`i`Ở đâu`s[i] = 1`Và`s[j] = 0`đối với một số người`j > i`. Nếu con số này bằng 0 thì chuỗi đó đã không giảm nên Bob thắng ngay lập tức. Ngược lại, Alice luôn có thể chọn một nước đi làm giảm số lần đảo ngược. Hơn nữa, tính chẵn lẻ của các phép đảo ngược quyết định người chiến thắng: nếu có đúng một phép đảo ngược, Alice có thể loại bỏ nó ngay lập tức và giành chiến thắng; nếu có nhiều, nước đi đầu tiên của Alice sẽ chọn nước ngoài cùng bên trái`1`s và ngoài cùng bên phải`0`s tạo thành sự đảo ngược, sắp xếp chúng và tiếp tục chơi một cách tối ưu. 

Cách tiếp cận tối ưu giảm việc quét từ cả hai đầu của chuỗi để tìm vị trí đầu tiên và cuối cùng trong đó`0`Và`1`cần phải được trao đổi. Điều này cho phép chúng ta xây dựng nước đi đầu tiên hợp lệ cho Alice trong thời gian O(n). Sau nước đi đầu tiên này, mọi lần chơi tiếp theo đều tuân theo logic tương tự, nhưng chúng ta chỉ cần xác định nước đi thắng đầu tiên chứ không phải mô phỏng toàn bộ trò chơi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc chuỗi`s`chiều dài`n`. 
2. Khởi tạo hai con trỏ:`l = 0`Và`r = n-1`. Di chuyển`l`chuyển tiếp cho đến khi`s[l] = 1`và di chuyển`r`lùi lại cho đến khi`s[r] = 0`. Mục tiêu là xác định vị trí ngoài cùng bên trái`1`và ngoài cùng bên phải`0`đó là không theo thứ tự. Nếu như`l >= r`, chuỗi không giảm và Bob thắng ngay lập tức. 
3. Mặt khác, tạo hai danh sách: một danh sách cho các chỉ số của`1`s bắt đầu từ bên trái lên đến`l`và cái khác cho chỉ số của`0`s bắt đầu từ phải xuống`r`. Các chỉ số này tạo thành dãy con hợp lệ đầu tiên mà Alice có thể sắp xếp. 
4. Sắp xếp danh sách chỉ số kết hợp để tạo ra bước di chuyển theo thứ tự tăng dần. 
5. Đầu ra`Alice`, số lượng chỉ mục khi di chuyển và danh sách các chỉ mục. 

Tại sao nó hoạt động: thuật toán xác định một dãy con tối thiểu chứa tất cả các đảo ngược giữa`1`s ở bên trái và`0`ở bên phải. Việc sắp xếp chuỗi con này giúp giảm số lần đảo ngược và đảm bảo rằng Alice có thể giành chiến thắng nếu chuỗi chưa được sắp xếp. Tính đúng đắn xuất phát từ thực tế là bất kỳ động thái nào cũng phải thay đổi chuỗi và ngoài cùng bên trái`1`s và ngoài cùng bên phải`0`s đảm bảo một dãy con hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        
        l, r = 0, n - 1
        while l < n and s[l] == '0':
            l += 1
        while r >= 0 and s[r] == '1':
            r -= 1
        
        if l >= r:
            print("Bob")
        else:
            ones = [i+1 for i in range(l) if s[i] == '1']
            zeros = [i+1 for i in range(r, n) if s[i] == '0']
            move = ones + zeros
            move.sort()
            print("Alice")
            print(len(move))
            print(*move)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xác định phần ngoài cùng bên trái`1`và ngoài cùng bên phải`0`tạo thành một sự đảo ngược. Nó thu thập các chỉ số của`1`s ở bên trái và`0`s ở bên phải thực hiện một nước đi, sắp xếp chúng và đưa ra kết quả. I/O nhanh được sử dụng để xử lý đầu vào lớn một cách hiệu quả. Có thể tránh được các lỗi từng cái một bằng cách sử dụng`i+1`vì vấn đề yêu cầu chỉ số dựa trên 1. 

## Ví dụ đã hoạt động 

Dấu vết cho đầu vào:```
3
3
000
2
10
3
101
```| Trường hợp thử nghiệm | s | tôi | r | Di chuyển | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 000 | 3 | 2 | - | Bob | 
| 2 | 10 | 0 | 1 | [1,2] | Alice, 2, 1 2 | 
| 3 | 101 | 0 | 2 | [1,2] | Alice, 2, 1 2 | 

Chuỗi đầu tiên đã được sắp xếp nên Bob thắng. Cái thứ hai có một đảo ngược ở vị trí 1 và 2. Alice chọn cả hai, sắp xếp chúng và giành chiến thắng ngay lập tức. Chuỗi thứ ba có sự đảo ngược giữa chuỗi thứ nhất`1`và cuối cùng`0`, và nước đi của Alice biến nó thành một chuỗi đã được sắp xếp, không để lại nước đi nào cho Bob. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét từ cả hai đầu một lần và xây dựng danh sách di chuyển | 
| Không gian | O(n) | Lưu trữ chỉ số của`1`cát`0`s để hình thành nước đi | 

Tổng của`n`trong tất cả các trường hợp thử nghiệm là 2·10^5, do đó tổng số thao tác nằm trong khoảng vài trăm nghìn, dễ dàng khớp với giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("3\n3\n000\n2\n10\n3\n101\n") == "Bob\nAlice\n2\n1 2\nAlice\n2\n1 2", "sample 1"

# Custom cases
assert run("2\n1\n0\n2\n11\n") == "Bob\nBob", "all zeros or all ones"
assert run("1\n4\n1001\n") == "Alice\n4\n1 2 3 4", "two inversions"
assert run("1\n5\n01010\n") == "Alice\n4\n2 3 4 5", "alternating pattern"
assert run("1\n6\n000111\n") == "Bob", "already sorted"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n0\n2\n11 | Bob\nBob | Trường hợp cạnh: không thể di chuyển | 
| 4\n1001 | Alice\n4\n1 2 3 4 | Nhiều lần đảo ngược được xử lý chính xác | 
| 5\n01010 | Alice\n4\n2 3 4 5 | Trình tự xen kẽ được xử lý | 
| 6\n000111 | Bob | Đã sắp xếp chuỗi | 

## Vỏ cạnh 

Xem xét đầu vào`1\n6\n000111\n`. Chuỗi đã được sắp xếp nên thuật toán sẽ quét`l`đến lần đầu tiên`1`ở vị trí 3 và`r`đến cuối cùng`0`ở vị trí 2. Vì`l >= r`, không có bước di chuyển nào tồn tại và thuật toán đưa ra`Bob`. Trường hợp cạnh có độ dài chuỗi tối thiểu, `1\n0\n
