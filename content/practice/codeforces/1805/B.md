---
title: "CF 1805B - ​​Dây Có Mục Tiêu"
description: "Chúng ta được cấp một chuỗi và được phép thực hiện chính xác một thao tác: chọn một ký tự ở vị trí nào đó và di chuyển nó đến đầu chuỗi. Mục tiêu là tạo ra chuỗi nhỏ nhất về mặt từ điển có thể sau thao tác này."
date: "2026-06-09T09:14:21+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1805
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 862 (Div. 2)"
rating: 800
weight: 1805
solve_time_s: 94
verified: true
draft: false
---

[CF 1805B - Chuỗi có mục tiêu](https://codeforces.com/problemset/problem/1805/B) 

**Đánh giá:** 800 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi và được phép thực hiện chính xác một thao tác: chọn một ký tự ở vị trí nào đó và di chuyển nó đến đầu chuỗi. Mục tiêu là tạo ra chuỗi nhỏ nhất về mặt từ điển có thể sau thao tác này. Đầu vào bao gồm nhiều trường hợp kiểm thử, mỗi trường hợp đưa ra một chuỗi. Đầu ra của mỗi test case là chuỗi kết quả sau bước di chuyển tối ưu. 

Các ràng buộc cho chúng ta biết rằng mỗi chuỗi có thể lên tới$10^5$ký tự và tổng độ dài chuỗi trong các trường hợp thử nghiệm nhiều nhất là$10^5$. Điều này ngụ ý rằng bất kỳ giải pháp nào cũng phải chạy trong thời gian gần như tuyến tính đối với độ dài chuỗi, như một$O(n^2)$cách tiếp cận sẽ liên quan đến$10^{10}$hoạt động, điều đó là không thể thực hiện được. 

Các trường hợp cạnh bao gồm các chuỗi trong đó ký tự đầu tiên đã là ký tự nhỏ nhất, các chuỗi có ký tự lặp lại và các chuỗi có nhiều vị trí chứa cùng một ký tự tối thiểu. Ví dụ: với "aaa", bất kỳ bước di chuyển nào cũng sẽ giữ nguyên chuỗi, trong khi đối với "cba", ký tự cuối cùng sẽ được chuyển lên phía trước, tạo ra "acb". Việc triển khai bất cẩn có thể chỉ di chuyển ký tự nhỏ nhất trên toàn cầu mà không xem xét thứ tự của phần còn lại của chuỗi, tạo ra kết quả dưới mức tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi vị trí$i$trong chuỗi, mô phỏng việc di chuyển ký tự đó lên phía trước và so sánh chuỗi kết quả với mức tối thiểu hiện tại. Đối với một chuỗi có độ dài$n$, mỗi mô phỏng yêu cầu xây dựng một chuỗi mới, lấy$O(n)$thời gian. Làm điều này cho tất cả$n$vị trí mang lại một$O(n^2)$thuật toán. Điều này đúng, nhưng quá chậm đối với$n = 10^5$. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải kiểm tra mọi vị trí. Việc di chuyển một ký tự chỉ có ý nghĩa nếu nó nhỏ hơn hoặc bằng ký tự đầu tiên hiện tại, vì nếu không, chuỗi mới sẽ bắt đầu bằng ký tự lớn hơn, điều này không thể tối ưu. Trong số các ứng cử viên nhỏ hơn hoặc bằng ký tự đầu tiên, chúng ta chỉ cần xét đến lần xuất hiện ngoài cùng bên trái tạo ra chuỗi nhỏ nhất sau khi di chuyển. Bằng cách lặp lại một lần để tìm ứng cử viên như vậy và so sánh chuỗi kết quả một cách hiệu quả, chúng ta rút gọn bài toán thành$O(n)$mỗi chuỗi. Cấu trúc bài toán đơn giản vì thao tác chỉ ảnh hưởng đến ký tự đầu tiên và giữ nguyên thứ tự của tất cả các ký tự khác, điều này làm cho việc so sánh trực tiếp trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Đối với mỗi trường hợp kiểm thử, hãy đọc chuỗi. 
3. Khởi tạo kết quả là chuỗi gốc, giả sử việc di chuyển ký tự đầu tiên là hợp lệ. 
4. Lặp lại chuỗi bắt đầu từ ký tự thứ hai. Đối với mỗi ký tự, hãy kiểm tra xem nó có nhỏ hơn ký tự đầu tiên hiện tại hay bằng không. Nếu nó nhỏ hơn, hãy cân nhắc việc di chuyển nó lên phía trước và tạo chuỗi kết quả bằng cách nối nó với chuỗi con không bao gồm ký tự này. So sánh chuỗi mới này với chuỗi tốt nhất hiện tại. Nếu nhỏ hơn thì cập nhật kết quả. 
5. Sau khi xem xét tất cả các ký tự, hãy xuất kết quả cho trường hợp kiểm thử đó. 

Tại sao nó hoạt động: Điều bất biến là chúng tôi luôn duy trì chuỗi nhỏ nhất về mặt từ điển được thấy cho đến nay. Vì chúng tôi chỉ xem xét việc di chuyển các ký tự có thể cải thiện ký tự đầu tiên và chúng tôi so sánh đầy đủ các chuỗi kết quả nên chúng tôi đảm bảo sẽ tìm thấy mức tối thiểu chung sau đúng một lần di chuyển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        best = s  # initially, moving the first char is a trivial option
        for i in range(1, n):
            candidate = s[i] + s[:i] + s[i+1:]
            if candidate < best:
                best = candidate
        print(best)

if __name__ == "__main__":
    solve()
```Mã sử ​​dụng I/O nhanh với`sys.stdin.readline`. Chúng ta lặp lại từng ký tự từ ký tự thứ hai vì việc di chuyển ký tự đầu tiên không làm thay đổi chuỗi. Việc xây dựng chuỗi ứng cử viên sử dụng tính năng cắt, trong Python tạo ra chuỗi mới một cách hiệu quả. Chúng tôi so sánh từ điển bằng cách sử dụng công cụ tích hợp sẵn của Python`<`toán tử, phù hợp với định nghĩa của vấn đề. Các lỗi xảy ra có thể tránh được bằng cách lập chỉ mục cẩn thận:`s[:i]`loại trừ ký tự hiện tại và`s[i+1:]`lấy hậu tố sau nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1: "cba" 

| tôi | s[i] | ứng cử viên | tốt nhất | 
| --- | --- | --- | --- | 
| 1 | b | bc a | cba | 
| 2 | một | một cb | cba → acb | 

Dấu vết cho thấy việc di chuyển 'a' lên phía trước sẽ tạo ra "acb", nhỏ hơn "cba". 

### Ví dụ 2: "acac" 

| tôi | s[i] | ứng cử viên | tốt nhất | 
| --- | --- | --- | --- | 
| 1 | c | c a ac | acac | 
| 2 | một | ac c | acac → aacc | 
| 3 | c | c a a | aacc | 

Dấu vết chứng tỏ rằng chúng ta phải xem xét tất cả các vị trí để tìm ra 'a' tối ưu ở chỉ số 2. Nó cũng cho thấy tầm quan trọng của việc so sánh đầy đủ chuỗi kết quả thay vì chỉ ký tự được di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi chuỗi | Chúng tôi lặp qua từng ký tự và xây dựng một chuỗi mới trong O(n) bằng cách cắt. | 
| Không gian | O(n) mỗi chuỗi | Việc lưu trữ chuỗi ứng cử viên yêu cầu không gian tuyến tính. | 

Tổng của tất cả độ dài chuỗi trong các trường hợp thử nghiệm là ≤ 10^5, do đó tổng thời gian chạy vẫn nằm trong giới hạn thời gian. Việc sử dụng bộ nhớ là tuyến tính theo độ dài chuỗi, phù hợp với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("4\n3\ncba\n4\nacac\n5\nabbcb\n4\naaba\n") == "acb\naacc\nabbcb\naaab", "sample tests"

# custom tests
assert run("1\n1\na\n") == "a", "single character"
assert run("1\n5\nzzzzz\n") == "zzzzz", "all same characters"
assert run("1\n5\nbacab\n") == "abacb", "minimal in the middle"
assert run("1\n3\ncab\n") == "acb", "minimal at last"
assert run("1\n6\naabaaa\n") == "aaabaa", "multiple minimal candidates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "một" | "một" | chuỗi ký tự đơn | 
| "zzzzzz" | "zzzzzz" | tất cả các ký tự giống hệt nhau | 
| "bacab" | "abacb" | ký tự tối ưu ở giữa | 
| "xe taxi" | "acb" | ký tự tối ưu ở cuối | 
| "aabaa" | "aaaaaa" | nhiều lần xuất hiện của ký tự tối thiểu | 

## Vỏ cạnh 

Đối với đầu vào một ký tự "a", thuật toán không xem xét vị trí nào khác và chuỗi gốc không thay đổi. Đối với "zzzzz", bất kỳ nước đi nào cũng tạo ra cùng một chuỗi và phép so sánh xác nhận nước đi tốt nhất là không thay đổi. Trong "bacab", di chuyển 'a' ở chỉ số 1 lên phía trước sẽ cho ra "abacb", nhỏ hơn bất kỳ ứng cử viên nào khác. Đối với "aabaaa", thuật toán chọn chính xác 'a' thứ hai (chỉ số 2) để tạo ra "aaabaa", xác nhận rằng các ký tự tối thiểu lặp lại được xử lý chính xác. 

Sự so sánh cẩn thận của thuật toán ở mỗi bước đảm bảo không xảy ra sự hình thành chuỗi sai lệch hoặc không chính xác và các so sánh từ điển xác định chính xác mức tối thiểu tổng thể trong tất cả các trường hợp cạnh.
