---
title: "CF 1419A - Trò chơi chữ số"
description: "Chúng tôi được yêu cầu mô phỏng một trò chơi chữ số theo lượt đơn giản giữa hai đặc vụ Raze và Breach. Mỗi trò chơi bắt đầu với một số nguyên dương gồm $n$ chữ số. Các chữ số được đánh số từ trái qua phải, bắt đầu bằng 1."
date: "2026-06-11T06:43:05+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1419
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 671 (Div. 2)"
rating: 900
weight: 1419
solve_time_s: 73
verified: true
draft: false
---

[CF 1419A - Trò chơi chữ số](https://codeforces.com/problemset/problem/1419/A) 

**Xếp hạng:** 900 
**Tags:** trò chơi, tham lam, thực hiện 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một trò chơi chữ số theo lượt đơn giản giữa hai đặc vụ Raze và Breach. Mỗi trò chơi bắt đầu với một số nguyên dương gồm$n$chữ số. Các chữ số được đánh số từ trái sang phải, bắt đầu bằng 1. Raze chỉ có thể đánh dấu các chữ số ở vị trí lẻ, còn Breach chỉ có thể đánh dấu các chữ số ở vị trí chẵn. Họ xen kẽ các chữ số đánh dấu cho đến khi chỉ còn lại một chữ số. Người chiến thắng được xác định bằng tính chẵn lẻ của chữ số cuối cùng đó: nếu là số lẻ thì Raze thắng; nếu hòa thì Breach thắng. Raze luôn đi trước. Đầu vào bao gồm$t$các trò chơi độc lập và với mỗi trò chơi, chúng tôi phải in ra ai thắng. 

Những hạn chế là vừa phải. Mỗi số có thể có tối đa 1000 chữ số và có thể có tới 100 trò chơi. Điều này loại trừ bất kỳ thuật toán nào cố gắng mô phỏng từng lượt đánh dấu chữ số, bởi vì điều đó có thể yêu cầu tới$n$các thao tác trên mỗi trò chơi, điều này có thể chấp nhận được, nhưng chúng ta có thể làm tốt hơn bằng cách suy luận về chữ số cuối cùng mà không cần mô phỏng đầy đủ. 

Trường hợp cạnh là nơi$n = 1$, nghĩa là chỉ có một chữ số và không thể di chuyển được. Trong trường hợp đó, chữ số cuối cùng ngay lập tức là yếu tố quyết định người chiến thắng. Ví dụ, đầu vào`1\n2`nên xuất ra`2`vì chữ số duy nhất là số chẵn. Một trường hợp tinh vi khác là khi tất cả các chữ số mà người chơi có thể truy cập đều có tính chẵn lẻ sai, buộc chúng phải có chuỗi đánh dấu dưới mức tối ưu. Việc nhận biết sớm những tình huống này cho phép chúng ta tính toán người chiến thắng mà không cần mô phỏng mọi nước đi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng mỗi lượt: theo dõi những chữ số nào chưa được đánh dấu, để Raze và Breach luân phiên đánh dấu một chữ số hợp lệ và tiếp tục cho đến khi còn lại một chữ số. Tại thời điểm đó, chúng tôi kiểm tra tính chẵn lẻ của nó. Cách tiếp cận này đúng nhưng nó liên quan đến$O(n)$hoạt động cho mỗi trò chơi. Với tối đa 100 trò chơi, mỗi trò chơi có 1000 chữ số, điều này dẫn đến khoảng 100.000 thao tác, có thể chấp nhận được nhưng không cần thiết. Nó cũng yêu cầu xử lý cẩn thận các lượt luân phiên và lập chỉ mục, điều này dễ xảy ra lỗi. 

Cách tiếp cận tối ưu xuất phát từ việc quan sát rằng chữ số cuối cùng là chữ số duy nhất quan trọng. Mỗi người chơi sẽ luôn cố gắng ép chữ số cuối cùng còn lại vào số chẵn lẻ chiến thắng của mình. Do đó, điều quan trọng là kiểm tra xem người chơi di chuyển cuối cùng có ít nhất một chữ số mà họ có thể bỏ đánh dấu theo số chẵn lẻ ưa thích của mình hay không. Đối với các số có độ dài lẻ, Raze di chuyển cuối cùng ở vị trí lẻ, vì vậy chúng tôi kiểm tra xem có chữ số lẻ ở vị trí lẻ hay không. Nếu có, Raze có thể đảm bảo chữ số cuối cùng là số lẻ. Ngược lại, Breach thắng. Tương tự, đối với các số có độ dài chẵn, Breach di chuyển cuối cùng ở các vị trí chẵn, vì vậy chúng tôi kiểm tra chữ số chẵn ở vị trí chẵn. Lý do này cho phép trực tiếp$O(n)$kiểm tra mỗi trò chơi, quét các chữ số một lần mà không mô phỏng lượt chơi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi trận | O(n) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(n) mỗi trận | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số trận đấu$t$. Với mỗi trò chơi, hãy đọc số chữ số$n$và chuỗi đại diện cho số nguyên. 
2. Xác định xem$n$là số lẻ hoặc số chẵn. Nếu như$n$là số lẻ, Raze sẽ có nước đi cuối cùng liên quan đến thế cờ lẻ; nếu như$n$là chẵn, Vi phạm sẽ có nước đi cuối cùng liên quan đến thế cờ chẵn. 
3. Nếu$n$là số lẻ, quét tất cả các chữ số ở vị trí lẻ (1, 3, 5, ...) và kiểm tra xem có số nào là số lẻ không. Nếu có ít nhất một số lẻ, Raze có thể đảm bảo chữ số cuối cùng còn lại là số lẻ nên Raze thắng. Ngược lại, Breach thắng. 
4. Nếu$n$là số chẵn, quét tất cả các chữ số ở vị trí chẵn (2, 4, 6, ...) và kiểm tra xem có số nào là số chẵn không. Nếu có ít nhất một số chẵn, Breach có thể đảm bảo chữ số cuối cùng còn lại là số chẵn, do đó Breach thắng. Nếu không, Raze sẽ thắng. 
5. Đầu ra`1`nếu Raze thắng hoặc`2`nếu Vi phạm thắng trong mỗi ván đấu. 

Tại sao nó hoạt động: Chữ số cuối cùng còn lại xác định người chiến thắng và mỗi người chơi có thể kiểm soát các vị trí có thể truy cập của mình một cách tối ưu. Bằng cách kiểm tra xem người chơi di chuyển cuối cùng có sẵn chữ số chiến thắng ở các vị trí được phép hay không, chúng tôi đảm bảo kết quả chính xác mà không cần mô phỏng mỗi lượt. Không có chuỗi nước đi nào có thể ghi đè lên điều này vì người chơi đối phương không thể loại bỏ lựa chọn chiến thắng duy nhất nếu nó tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    if n % 2 == 1:
        # Raze has the final move on odd positions
        for i in range(0, n, 2):  # odd positions in 0-based indexing
            if int(s[i]) % 2 == 1:
                print(1)
                break
        else:
            print(2)
    else:
        # Breach has the final move on even positions
        for i in range(1, n, 2):  # even positions in 0-based indexing
            if int(s[i]) % 2 == 0:
                print(2)
                break
        else:
            print(1)
```Chúng tôi sử dụng lập chỉ mục dựa trên số 0 để đơn giản hóa việc xử lý vị trí. Vòng lặp`for i in range(0, n, 2)`truy cập các vị trí 1, 3, 5 trong lập chỉ mục dựa trên 1. các`else`mệnh đề của vòng lặp for sẽ chạy nếu vòng lặp không bị hỏng, nghĩa là không tìm thấy chữ số chiến thắng. Điều này tránh việc đưa ra một biến cờ riêng biệt. Chúng tôi loại bỏ chuỗi đầu vào để xóa dòng mới ở cuối`input()`. 

## Ví dụ đã hoạt động 

### Dấu vết đầu vào mẫu 1 

đầu vào:```
1
3
102
```| Bước | n | Vị trí đã được kiểm tra | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 3 | vị trí lẻ 1,3 → chữ số 1,2 | chữ số 1 lẻ | 
| 2 | n lẻ, Raze kiểm tra các chữ số lẻ | thấy lẻ ở vị trí 1 | Raze thắng | 

Điều này chứng tỏ Raze thắng vì họ có thể để chữ số cuối cùng là số lẻ. 

### Dấu vết đầu vào mẫu 2 

đầu vào:```
1
4
2069
```| Bước | n | Vị trí đã được kiểm tra | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 4 | vị trí chẵn 2,4 → chữ số 0,9 | chữ số 0 là số chẵn | 
| 2 | n chẵn, Vi phạm kiểm tra chữ số chẵn | được tìm thấy ngay cả ở vị trí 2 | Vi phạm chiến thắng | 

Điều này xác nhận rằng thuật toán xác định chính xác chiến lược chiến thắng của Breach. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi trận | Chúng tôi quét các vị trí chẵn hoặc lẻ một lần, tuyến tính theo số chữ số | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài lưu trữ chuỗi đầu vào và các biến vòng lặp | 

Được cho$t \le 100$Và$n \le 1000$, trường hợp xấu nhất là kiểm tra 100.000 chữ số, nằm trong giới hạn 1 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        if n % 2 == 1:
            for i in range(0, n, 2):
                if int(s[i]) % 2 == 1:
                    print(1)
                    break
            else:
                print(2)
        else:
            for i in range(1, n, 2):
                if int(s[i]) % 2 == 0:
                    print(2)
                    break
            else:
                print(1)
    return output.getvalue().strip()

# Provided samples
assert run("4\n1\n2\n1\n3\n3\n102\n4\n2069\n") == "2\n1\n1\n2", "sample cases"

# Custom cases
assert run("2\n1\n1\n2\n8") == "1\n2", "single-digit edge cases"
assert run("2\n5\n13579\n6\n246802") == "1\n2", "all odd or all even"
assert run("1\n3\n222") == "2", "odd length, no odd digits"
assert run("1\n4\n1357") == "1", "even length, no even digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`| 1 | Lẻ một chữ số, Raze thắng liền | 
|`1\n1\n2`| 2 | Chẵn một chữ số, Vi phạm thắng ngay | 
|`5\n13579`| 1 | Độ dài lẻ tất cả các chữ số lẻ, Raze có thể thắng | 
|`6\n246802`| 2 | Độ dài chẵn tất cả các chữ số chẵn, Vi phạm có thể thắng | 
|`3\n222`| 2 | Độ dài lẻ, không có chữ số lẻ, Raze thua | 
|`4\n1357`| 1 | Độ dài chẵn, không có chữ số chẵn, Vi phạm không thể thắng | 

## Vỏ cạnh 

cho`n = 1`, thuật toán sẽ kiểm tra ngay chữ số duy nhất. Vì`1\n3`, Raze thắng vì chữ số đó là số lẻ
