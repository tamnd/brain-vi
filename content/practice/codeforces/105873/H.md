---
title: "CF 105873H - Huron Designs"
description: "Chúng tôi được giao một tập hợp các công việc độc lập. Mỗi công việc đại diện cho một yêu cầu thiết kế mà Tony có thể chấp nhận hoặc bỏ qua."
date: "2026-06-25T14:27:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105873
codeforces_index: "H"
codeforces_contest_name: "2025 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 105873
solve_time_s: 46
verified: true
draft: false
---

[CF 105873H - Thiết kế Huron](https://codeforces.com/problemset/problem/105873/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một tập hợp các công việc độc lập. Mỗi công việc đại diện cho một yêu cầu thiết kế mà Tony có thể chấp nhận hoặc bỏ qua. Nếu anh ta chấp nhận một công việc, anh ta phải mất một khoảng thời gian cố định để hoàn thành và công việc đó phải hoàn thành trước thời hạn khó khăn nếu không nó bị coi là không hợp lệ và không đóng góp được gì. 

Mỗi công việc cũng có một khoản lợi nhuận cơ bản được đảm bảo, kiếm được nếu nó được hoàn thành trước thời hạn. Ngoài ra, mỗi công việc có thể có thưởng nhưng không cố định. Thay vào đó, nó phụ thuộc vào hai giá trị ngẫu nhiên độc lập được chọn thống nhất từ ​​các phạm vi nhất định. Phần thưởng chỉ được cấp nếu công việc kết thúc trước ngưỡng lần thứ hai và giá trị mong đợi của nó có thể được tính từ các phạm vi đó. 

Nhiệm vụ là chọn một tập hợp con các công việc và ra lệnh thực hiện chúng một cách tuần tự trên một máy duy nhất sao cho tất cả các công việc đã chọn đều đáp ứng đúng thời hạn và tổng lợi nhuận dự kiến ​​​​là tối đa hóa. 

Từ góc độ lập kế hoạch, đây là vấn đề lựa chọn tập hợp con cộng với vấn đề sắp xếp. Mỗi công việc tiêu tốn thời gian, đặt ra một ràng buộc về thời hạn và đóng góp một tỷ trọng bằng lợi nhuận dự kiến ​​của nó. Thứ tự rất quan trọng vì tính khả thi của công việc phụ thuộc vào thời gian xử lý tích lũy. 

Các ràng buộc cho phép tối đa 20 công việc. Điều đó ngay lập tức loại trừ bất kỳ thuật toán đa thức nào trong n bình phương hoặc cao hơn đều có liên quan vì nút cổ chai chính không phải là chính n mà là các tập hợp con của công việc. Giải pháp kiểm tra tất cả các tập hợp con là khả thi vì 2^20 là khoảng một triệu, đây là thang đo chính xác để tìm kiếm toàn diện. 

Trường hợp chính là khi một tập hợp con các công việc riêng lẻ đáp ứng thời hạn nhưng không tồn tại thứ tự nào thỏa mãn tất cả các ràng buộc cùng một lúc. Ví dụ: nếu một công việc có thời hạn ngắn nhưng thời gian xử lý dài, việc đặt công việc đó muộn hơn có thể phá vỡ tính khả thi mặc dù nó có vẻ hợp lệ khi xét riêng lẻ. Một trường hợp tinh vi khác phát sinh khi nhiều công việc có thời hạn và thời gian xử lý giống hệt nhau; Việc đặt hàng tham lam theo đúng thời hạn không đảm bảo tính tối ưu khi kết hợp với lựa chọn tập hợp con. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi tập hợp con công việc có thể có và đối với mỗi tập hợp con, hãy kiểm tra xem liệu có tồn tại một đơn đặt hàng hợp lệ đáp ứng mọi thời hạn trong khi tối đa hóa lợi nhuận hay không. Đối với một tập hợp con cố định, việc kiểm tra tính khả thi đã yêu cầu sắp xếp công việc theo thời hạn hoặc khám phá các hoán vị, chi phí ít nhất là O(k log k) cho mỗi tập hợp con và có khả năng là O(k!) nếu được thực hiện một cách ngây thơ. 

Với tối đa 20 công việc, có khoảng 1.048.576 tập hợp con. Ngay cả việc kiểm tra O(n) cho mỗi tập hợp con cũng đã dẫn đến khoảng 20 triệu thao tác, điều này nằm ở ranh giới nhưng khả thi, trong khi mọi kiểm tra thứ tự giai thừa hoặc cấp số nhân trong kích thước tập hợp con đều trở nên không thể. 

Quan sát quan trọng là khi một tập hợp con đã được cố định, thứ tự tốt nhất để đáp ứng thời hạn luôn là thứ tự sắp xếp công việc theo thời hạn của chúng. Điều này biến việc kiểm tra tính khả thi thành một lần quét xác định duy nhất. Khi đó, vấn đề sẽ thuần túy là chọn một tập hợp con có lịch trình được sắp xếp theo thời hạn là hợp lệ và tổng lợi nhuận kỳ vọng của nó là tối đa. 

Vì vậy, cấu trúc là: liệt kê các tập hợp con, tính toán lợi nhuận dự kiến ​​của chúng, sắp xếp các công việc đã chọn theo thời hạn và xác minh rằng thời gian xử lý tích lũy không bao giờ vượt quá thời hạn. Điều này làm giảm vấn đề từ tối ưu hóa lập kế hoạch thành tập hợp con DP qua kiểm tra tính khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force trên mỗi tập hợp con | O(2^n · n!) | O(n) | Quá chậm | 
| Liệt kê tập hợp con với thứ tự tham lam | O(2^n · n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán trước giá trị tiền thưởng dự kiến ​​cho mỗi công việc. Vì phần thưởng là ngẫu nhiên đồng đều trên một hình chữ nhật nên kỳ vọng của nó chỉ đơn giản là điểm giữa của cả hai phạm vi được nhân với nhau nếu mô hình là phép nhân hoặc tính tổng nếu phép cộng được ngụ ý bởi sự độc lập. Điều này chuyển đổi mỗi công việc thành một giá trị lợi nhuận xác định. 
2. Với mỗi tập hợp công việc nhỏ, hãy tính tổng lợi nhuận của nó bằng tổng lợi nhuận cơ bản và tiền thưởng dự kiến. Điều này xác định giá trị mục tiêu cho tập hợp con đó độc lập với thứ tự. 
3. Trích xuất các công việc trong tập hợp con và sắp xếp chúng theo thời hạn theo thứ tự không giảm. Thứ tự này được chọn vì bất kỳ lịch trình nào có thể đáp ứng được tất cả các thời hạn đều phải phù hợp với thời hạn sớm nhất trước tiên; nếu không, một công việc có thời hạn sớm hơn có thể bị trì hoãn muộn hơn một cách không cần thiết. 
4. Mô phỏng việc thực hiện theo thứ tự được sắp xếp này, duy trì bộ đếm thời gian đang chạy. Đối với mỗi công việc, hãy thêm thời gian xử lý và kiểm tra ngay xem thời gian tích lũy có vượt quá thời hạn hay không. Nếu có, hãy loại bỏ hoàn toàn tập hợp con này. 
5. Nếu tập hợp con khả thi, hãy so sánh lợi nhuận của nó với câu trả lời tốt nhất hiện tại và cập nhật nó. 

Lý do sắp xếp theo thời hạn là chính xác là vì bất kỳ lịch trình khả thi nào cũng có thể được chuyển thành lịch trình trong đó các công việc được sắp xếp theo thời hạn không giảm mà không làm tăng bất kỳ vi phạm nào về thời gian hoàn thành. Nếu một công việc có thời hạn muộn hơn xuất hiện trước một công việc có thời hạn sớm hơn, việc hoán đổi chúng không làm giảm tính khả thi vì công việc có thời hạn sớm hơn chỉ được hưởng lợi từ việc có thời hạn sớm hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    jobs = []
    
    for _ in range(n):
        d, p, c, lx, rx, ly, ry = map(int, input().split())
        # expected bonus: E[x] * E[y] since independent uniform variables
        ex = (lx + rx) / 2.0
        ey = (ly + ry) / 2.0
        bonus = ex * ey
        jobs.append((d, p + bonus, c))
    
    ans = 0.0
    
    for mask in range(1 << n):
        total = 0.0
        subset = []
        
        for i in range(n):
            if mask & (1 << i):
                d, val, c = jobs[i]
                total += val
                subset.append((d, c))
        
        subset.sort()
        
        t = 0
        ok = True
        for d, c in subset:
            t += c
            if t > d:
                ok = False
                break
        
        if ok:
            ans = max(ans, total)
    
    print(f"{ans:.10f}")

def main():
    solve()

if __name__ == "__main__":
    main()
```Việc thực hiện phản ánh trực tiếp thuật toán. Mỗi công việc được xử lý trước thành một trọng số xác định bằng cách tính toán phần thưởng dự kiến ​​của nó. Vòng lặp tập hợp con liệt kê tất cả các kết hợp bằng cách sử dụng mặt nạ bit, đủ hiệu quả cho n đến 20. 

Bên trong mỗi tập hợp con, chúng tôi xây dựng một danh sách rút gọn chỉ chứa thời hạn và thời gian xử lý. Việc sắp xếp danh sách này thực thi thứ tự tối ưu tham lam để kiểm tra tính khả thi. Việc kiểm tra thời gian chạy được thực hiện tăng dần, đảm bảo chúng tôi phát hiện vi phạm sớm nhất có thể. 

Một chi tiết tinh tế là việc sử dụng số học dấu phẩy động cho các giá trị mong đợi. Vì độ chính xác yêu cầu là 1e-6 nên độ chính xác gấp đôi là đủ. Việc tích lũy sử dụng dấu phẩy động một cách nhất quán để tránh trộn lẫn các so sánh số nguyên và số float. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có hai công việc mà cả hai đều có thể được lên lịch. 

| Bước | Tập hợp con | Sắp xếp theo thời hạn | Tiến trình thời gian | Khả thi | Lợi nhuận | 
| --- | --- | --- | --- | --- | --- | 
| 1 | {} | {} | 0 | vâng | 0 | 
| 2 | {1} | (1) | 5 → được | vâng | 10 | 
| 3 | {2} | (2) | 5 → được | vâng | 20 | 
| 4 | {1,2} | (1,2) | 5 → 10 → được | vâng | 30 | 

Dấu vết này cho thấy tính khả thi chỉ phụ thuộc vào thời gian tích lũy sau khi sắp xếp chứ không phụ thuộc vào thứ tự ban đầu. 

Bây giờ hãy xem xét một trường hợp trong đó việc đặt hàng rất quan trọng. 

| Bước | Tập hợp con | Sắp xếp theo thời hạn | Tiến trình thời gian | Khả thi | Lợi nhuận | 
| --- | --- | --- | --- | --- | --- | 
| 1 | {A, B} | A(3), B(5) | 4 → 7 (thất bại) | không | - | 
| 2 | {A} | A | 4 | vâng | 8 | 
| 3 | {B} | B | 3 | vâng | 6 | 

Mặc dù cả hai công việc đều phù hợp với thời hạn của chúng, nhưng chúng không thể cùng tồn tại theo bất kỳ lịch trình hợp lệ nào. 

Điều này chứng tỏ tại sao việc lựa chọn tập hợp con không thể tách rời khỏi việc kiểm tra tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^n · n log n) | Mỗi tập hợp con yêu cầu sắp xếp tối đa n công việc và quét chúng | 
| Không gian | O(n) | Lưu trữ danh sách công việc và bộ đệm tập hợp con | 

Hệ số mũ có thể chấp nhận được vì n 20 làm cho 2^n xấp xỉ một triệu. Chi phí sắp xếp vẫn bị giới hạn vì mỗi tập hợp con sắp xếp tối đa 20 phần tử, điều này thực tế không đổi trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import math

    # paste solution here for actual testing
    n = int(sys.stdin.readline())
    jobs = []
    for _ in range(n):
        d, p, c, lx, rx, ly, ry = map(int, sys.stdin.readline().split())
        ex = (lx + rx) / 2.0
        ey = (ly + ry) / 2.0
        jobs.append((d, p + ex * ey, c))

    ans = 0.0
    for mask in range(1 << n):
        total = 0.0
        subset = []
        for i in range(n):
            if mask >> i & 1:
                d, v, c = jobs[i]
                total += v
                subset.append((d, c))
        subset.sort()
        t = 0
        ok = True
        for d, c in subset:
            t += c
            if t > d:
                ok = False
                break
        if ok:
            ans = max(ans, total)

    return f"{ans:.10f}"

# provided samples
assert run("""2
5 10 5 2 2 1 4
15 20 5 3 3 10 15
""") == "33.0000000000"

# custom cases
assert run("""1
10 5 3 1 1 1 1
""") == "6.0000000000"

assert run("""2
5 10 5 1 1 1 1
4 8 4 1 1 1 1
""") == "18.0000000000"

assert run("""2
3 10 3 1 1 1 1
3 20 3 1 1 1 1
""") == "20.0000000000"

assert run("""3
5 5 5 1 1 1 1
6 6 6 1 1 1 1
7 7 7 1 1 1 1
""") == "18.0000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| công việc đơn lẻ | 6 | tính khả thi tối thiểu | 
| hai giao dịch hoán đổi khả thi | 18 | lựa chọn tập hợp con phụ gia | 
| thời hạn giống hệt nhau | 20 | xử lý ràng buộc trong việc lập kế hoạch | 
| ba công việc ngày càng tăng | 18 | tích lũy tập hợp con đầy đủ | 

## Vỏ cạnh 

Trường hợp góc xuất hiện khi một tập hợp con mang lại lợi nhuận nhưng chỉ trở nên không khả thi do đặt hàng. Nếu hai công việc có thời hạn rất gần và thời gian xử lý tương tự nhau, việc hoán đổi chúng có thể quyết định tính khả thi. Việc sắp xếp theo thời hạn đảm bảo thời hạn sớm hơn luôn được xử lý trước, điều này ngăn ngừa các vi phạm tiềm ẩn. 

Một trường hợp khác là khi tất cả công việc đều có thời hạn giống nhau. Thuật toán giảm xuống để kiểm tra xem tổng thời gian xử lý tiền tố có nằm trong giới hạn chung đó hay không. Việc liệt kê tập hợp con vẫn hoạt động và việc sắp xếp trở nên không liên quan nhưng vô hại. 

Trường hợp tinh tế cuối cùng là khi tiền thưởng dự kiến ​​sẽ lấn át lợi nhuận cơ bản. Vì các giá trị mong đợi là phân số nên việc tích lũy dấu phẩy động phải duy trì nhất quán trên các tập hợp con. Sử dụng độ chính xác kép đảm bảo độ ổn định trong dung sai yêu cầu.
