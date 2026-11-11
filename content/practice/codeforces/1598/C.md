---
title: "CF 1598C - Xóa hai phần tử"
description: "Chúng ta được cung cấp một mảng và giá trị trung bình của nó được cố định cho mảng ban đầu. Nhiệm vụ là loại bỏ chính xác hai phần tử và đếm xem có thể loại bỏ bao nhiêu cặp chỉ số sao cho giá trị trung bình của các phần tử còn lại không thay đổi."
date: "2026-06-10T08:46:03+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "implementation", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1598
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 115 (Rated for Div. 2)"
rating: 1200
weight: 1598
solve_time_s: 86
verified: true
draft: false
---

[CF 1598C - Xóa hai phần tử](https://codeforces.com/problemset/problem/1598/C) 

**Đánh giá:** 1200 
**Tags:** cấu trúc dữ liệu, dp, triển khai, toán học, hai con trỏ 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và giá trị trung bình của nó được cố định cho mảng ban đầu. Nhiệm vụ là loại bỏ chính xác hai phần tử và đếm xem có thể loại bỏ bao nhiêu cặp chỉ số sao cho giá trị trung bình của các phần tử còn lại không thay đổi. 

Thay vì suy nghĩ trực tiếp về mặt trung bình, sẽ hữu ích hơn nếu chuyển điều kiện thành một ràng buộc tổng. Nếu mảng ban đầu có tổng$S$và kích thước$n$, thì trung bình là$k = S/n$. Sau khi loại bỏ hai phần tử$a_i$Và$a_j$, số tiền còn lại trở thành$S - a_i - a_j$, và kích thước còn lại là$n - 2$. Chúng tôi yêu cầu:$$\frac{S - a_i - a_j}{n - 2} = \frac{S}{n}$$Phương trình này mô tả đầy đủ những cặp nào hợp lệ. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số phần tử trên tất cả các trường hợp thử nghiệm, loại trừ mọi giải pháp bậc hai cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào thử trực tiếp tất cả các cặp sẽ yêu cầu tới$O(n^2)$hoạt động, vượt xa những gì có thể thực hiện được trong hai giây. 

Một vấn đề tế nhị xuất hiện khi xử lý giá trị trung bình: vì$k$có thể không phải là số nguyên, các phép so sánh số nguyên ngây thơ có thể phá vỡ tính chính xác nếu người ta cố gắng tính toán$k$và so sánh các giá trị dấu phẩy động. Một lỗi phổ biến khác là tính toán lại các khoản tiền nhiều lần trong các vòng lặp, điều này vô tình biến một$O(n)$ý tưởng vào$O(n^2)$. 

Trường hợp cạnh cụ thể là khi tất cả các phần tử đều bằng nhau. Ví dụ, trong`[5, 5, 5, 5]`, mọi cặp đều hoạt động, nhưng một dẫn xuất sai giả định một số “cấu trúc đặc biệt” có thể loại bỏ các cặp hợp lệ một cách không chính xác vì nó mong đợi sự thay đổi trong các giá trị. 

Một trường hợp khác là khi chỉ các cặp bổ sung rất cụ thể mới có tác dụng, chẳng hạn như`[50, 20, 10]`, trong đó việc loại bỏ bất kỳ cặp nào sẽ làm thay đổi giá trị trung bình, vì vậy câu trả lời là 0. Điều này nắm bắt các giải pháp giả định không chính xác về tính đối xứng hoặc phương pháp phỏng đoán dựa trên tần số mà không rút ra được điều kiện chính xác. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực thử từng cặp$(i, j)$, loại bỏ chúng, tính lại tổng và kiểm tra xem giá trị trung bình thu được có bằng giá trị trung bình ban đầu hay không. Tính lại số tiền sau mỗi lần loại bỏ chi phí$O(n)$, và có$O(n^2)$cặp, do đó tổng số sẽ trở thành$O(n^3)$. Ngay cả khi chúng ta tính trước tổng một lần, mỗi cặp vẫn yêu cầu kiểm tra điều kiện dẫn xuất, đưa ra$O(n^2)$, vốn đã trở nên quá lớn đối với$2 \cdot 10^5$tổng số phần tử. 

Quan sát quan trọng xuất phát từ việc viết lại điều kiện theo đại số. Bắt đầu từ:$$\frac{S - a_i - a_j}{n - 2} = \frac{S}{n}$$nhân chéo cho:$$n(S - a_i - a_j) = (n - 2)S$$Mở rộng:$$nS - n(a_i + a_j) = nS - 2S$$Đang hủy$nS$:$$n(a_i + a_j) = 2S$$Vì thế:$$a_i + a_j = \frac{2S}{n}$$Điều này làm giảm toàn bộ vấn đề về việc đếm các cặp có tổng bằng giá trị mục tiêu cố định. Điều đó ngay lập tức gợi ý một giải pháp tuyến tính dựa trên tần số: chúng tôi quét mảng trong khi theo dõi số lần mỗi giá trị xuất hiện và đối với mỗi phần tử, chúng tôi kiểm tra xem có bao nhiêu phần tử trước đó tạo thành tổng được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số tiền$S$của mảng và rút ra tổng mục tiêu$T = \frac{2S}{n}$. Nếu như$2S$không chia hết cho$n$, không có cặp hợp lệ nào tồn tại nên câu trả lời là 0. Việc kiểm tra tính chia hết này ngăn cản việc làm việc với các mục tiêu không phải là số nguyên không thể tương ứng với các phần tử mảng số nguyên. 
2. Tạo bản đồ băm để lưu trữ tần suất của các giá trị đã thấy cho đến nay. Điều này cho phép tra cứu liên tục theo thời gian để tìm phần bổ sung. 
3. Lặp lại mảng từ trái sang phải, xử lý từng phần tử$a[i]$là phần tử thứ hai của một cặp tiềm năng. 
4. Với mỗi phần tử, hãy tính phần bù cần thiết$T - a[i]$. Số cặp hợp lệ kết thúc ở chỉ mục này chính xác là số lần phần bù này đã xuất hiện trong tiền tố. 
5. Thêm số này vào câu trả lời, sau đó tăng tần số của phần tử hiện tại. 

Ý tưởng chính là mỗi cặp hợp lệ được tính chính xác một lần khi phần tử thứ hai của cặp được xử lý. 

### Tại sao nó hoạt động 

Phép biến đổi làm giảm điều kiện thành bài toán ghép tổng cố định. Mỗi cặp hợp lệ tương ứng duy nhất với một cặp chỉ số$(i, j)$với$i < j$Và$a_i + a_j = T$. Bản đồ tần số đảm bảo rằng khi xử lý chỉ mục$j$, chúng tôi đếm chính xác những$i < j$thỏa mãn phương trình. Không có cặp nào bị bỏ sót vì mọi chỉ mục đều được xử lý dưới dạng điểm cuối thứ hai đúng một lần và không có cặp nào được tính hai lần vì chúng tôi không bao giờ mong đợi trong mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        
        S = sum(arr)
        if (2 * S) % n != 0:
            print(0)
            continue
        
        target = (2 * S) // n
        
        freq = {}
        ans = 0
        
        for x in arr:
            need = target - x
            if need in freq:
                ans += freq[need]
            freq[x] = freq.get(x, 0) + 1
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tính tổng tổng và kiểm tra xem có tồn tại tổng cặp mục tiêu số nguyên hợp lệ hay không. Nếu không, nó ngay lập tức trả về 0, vì không có cặp số nguyên nào có thể đáp ứng yêu cầu phân số. 

Từ điển tần số lưu trữ số lần mỗi giá trị được nhìn thấy cho đến nay. Khi chúng tôi quét mảng, mỗi phần tử sẽ đóng góp vào câu trả lời dựa trên phần bổ sung đã thấy trước đó, đảm bảo thứ tự chính xác$i < j$. 

Việc sử dụng tần số tiền tố sẽ tránh được việc tính hai lần và giữ cho giải pháp tuyến tính cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
arr = [1, 4, 7, 3, 5]
```Chúng tôi tính toán:$S = 20$, Vì thế$T = 2S/n = 40/5 = 8$Chúng tôi theo dõi tần suất và đóng góp: 

| tôi | một [tôi] | tần số trước | cần = 8 - a[i] | đóng góp | tần số sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | {} | 7 | 0 | {1:1} | 
| 1 | 4 | {1:1} | 4 | 0 | {1:1,4:1} | 
| 2 | 7 | {1:1,4:1} | 1 | 1 | {1:1,4:1,7:1} | 
| 3 | 3 | ... | 5 | 0 | ... | 
| 4 | 5 | ... | 3 | 1 | ... | 

Tổng số câu trả lời là 2. 

Điều này cho thấy các cặp hợp lệ chính xác là các cặp tạo thành tổng 8 và mỗi cặp được tính khi gặp phần tử thứ hai. 

### Ví dụ 2 

đầu vào:```
n = 4
arr = [8, 8, 8, 8]
```Đây$S = 32$, Vì thế$T = 16$. Mỗi cặp phần tử có tổng bằng 16. 

| tôi | một [tôi] | tần số trước | cần | đóng góp | tần số sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 8 | {} | 8 | 0 | {8:1} | 
| 1 | 8 | {8:1} | 8 | 1 | {8:2} | 
| 2 | 8 | {8:2} | 8 | 2 | {8:3} | 
| 3 | 8 | {8:3} | 8 | 3 | {8:4} | 

Tổng cộng là$1 + 2 + 3 = 6$. 

Điều này chứng tỏ rằng tính bội số được xử lý một cách tự nhiên bằng cách tích lũy tần số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần với các phép toán băm O(1) | 
| Không gian |$O(n)$| Bản đồ tần số lưu trữ tối đa n giá trị riêng biệt | 

Tổng số tiền của$n$qua các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó việc quét tuyến tính trên tất cả các phần tử đầu vào diễn ra trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            arr = list(map(int, input().split()))
            S = sum(arr)
            if (2 * S) % n != 0:
                print(0)
                continue
            target = (2 * S) // n
            freq = {}
            ans = 0
            for x in arr:
                ans += freq.get(target - x, 0)
                freq[x] = freq.get(x, 0) + 1
            print(ans)

    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("""4
4
8 8 8 8
3
50 20 10
5
1 4 7 3 5
7
1 2 3 4 5 6 7
""") == "6\n0\n2\n3"

# all equal small
assert run("""1
3
5 5 5
""") == "3"

# impossible fractional target
assert run("""1
3
1 1 2
""") == "0"

# large symmetric
assert run("""1
6
1 2 3 4 5 6
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 3 | tất cả các cặp đếm hợp lệ | 
| giá trị trung bình phân số | 0 | bảo vệ phân chia | 
| mảng 1..6 | 3 | đếm cặp chính xác cho nhiều trận đấu | 

## Vỏ cạnh 

Đối với một mảng trong đó tất cả các giá trị đều giống hệt nhau, chẳng hạn như`[10, 10, 10]`, thuật toán tính toán mục tiêu bằng hai lần giá trị đó. Mọi phần tử đều khớp với điều kiện bù và việc tích lũy tần số sẽ tính tất cả các cặp chính xác một lần khi chỉ số thứ hai tăng lên. Đầu ra cuối cùng là$\binom{3}{2} = 3$, phù hợp với kết quả tổ hợp dự kiến. 

Đối với trường hợp tổng yêu cầu không phải là số nguyên, chẳng hạn như`[1, 1, 2]`, mục tiêu được tính toán trở thành không thể tách rời vì$2S/n$không thể chia được. The algorithm immediately returns zero, and no frequency processing occurs. This avoids any incorrect pairing attempts that would otherwise rely on impossible equality.
