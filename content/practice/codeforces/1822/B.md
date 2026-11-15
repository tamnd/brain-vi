---
title: "CF 1822B - Karina và Mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép xóa bất kỳ tập hợp con nào của các phần tử trong khi vẫn giữ nguyên thứ tự tương đối của những phần tử còn lại."
date: "2026-06-09T07:47:59+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1822
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 867 (Div. 3)"
rating: 800
weight: 1822
solve_time_s: 68
verified: true
draft: false
---

[CF 1822B - Karina và Mảng](https://codeforces.com/problemset/problem/1822/B) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép xóa bất kỳ tập hợp con nào của các phần tử trong khi vẫn giữ nguyên thứ tự tương đối của những phần tử còn lại. Sau khi xóa, chúng ta phải giữ lại ít nhất hai phần tử và đánh giá mảng kết quả bằng cách xem xét tất cả các cặp liền kề và lấy tích lớn nhất trong số chúng. Mục tiêu là chọn những phần tử cần giữ sao cho tích liền kề tối đa này càng lớn càng tốt. 

Chi tiết quan trọng là việc xóa cho phép chúng tôi “kết nối” bất kỳ hai phần tử còn sót lại nào ban đầu không liền kề nhau. Vì vậy, vấn đề không phải là về những người hàng xóm ban đầu mà là về việc chọn bất kỳ cặp chỉ số nào$i < j$trở nên liên tiếp sau khi loại bỏ mọi thứ ở giữa và tối đa hóa$a_i \cdot a_j$. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số phần tử trong các trường hợp thử nghiệm, điều này ngay lập tức loại trừ mọi phép khám phá bậc hai của tất cả các cặp hoặc tất cả các chuỗi con. Một giải pháp thử tất cả các cặp hoặc mô phỏng việc xóa một cách rõ ràng sẽ quá chậm vì một trường hợp thử nghiệm đơn lẻ có thể yêu cầu khoảng$10^{10}$hoạt động trong trường hợp xấu nhất. 

Chế độ lỗi tinh vi xuất hiện khi chỉ nghĩ về các phần tử liền kề trong mảng ban đầu. Ví dụ: trong một mảng như$[1, 100, 2]$, sản phẩm liền kề tốt nhất là$100$, nhưng sau khi xóa chúng ta có thể chọn$[1, 2]$để có được$2$, tệ hơn, hoặc$[100]$không hợp lệ. Tuy nhiên, các trường hợp thú vị hơn liên quan đến việc bỏ qua các phần tử để kết nối các giá trị lớn ở xa mà ban đầu không liền kề. 

Một trường hợp cạnh khác là số âm. Một cách tiếp cận ngây thơ chỉ xem xét các giá trị tuyệt đối lớn sẽ thất bại vì tích phụ thuộc vào dấu và tích cực đại có thể đến từ hai số âm. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: xem xét mọi tập con có kích thước ít nhất là hai, tính tích liền kề tốt nhất của nó và theo dõi mức tối đa. Ngay cả khi chúng tôi hạn chế kiểm tra các cặp sau khi xóa, chúng tôi vẫn xem xét hiệu quả tất cả các cặp chỉ mục$(i, j)$với$i < j$, vì bất kỳ cặp nào như vậy đều có thể được tạo liền kề bằng cách xóa các phần tử ở giữa. Với mỗi cặp, chúng tôi tính toán$a_i \cdot a_j$và lấy tối đa. 

Điều này ngay lập tức trở thành$O(n^2)$cho mỗi trường hợp thử nghiệm, quá lớn khi$n$đạt tới$2 \cdot 10^5$. 

Quan sát quan trọng là việc xóa cho phép chúng ta bỏ qua cấu trúc hoàn toàn. Bất kỳ hai phần tử nào được chọn đều có thể trở nên liền kề nhau, vì vậy câu trả lời cuối cùng chỉ đơn giản là tích lớn nhất trên tất cả các cặp$i < j$. Điều đó làm giảm vấn đề chọn cặp tốt nhất chứ không phải sắp xếp trình tự. 

Khi điều này được nhận ra, nhiệm vụ sẽ trở thành tìm tích lớn nhất của hai số trong một mảng. Sự tinh tế duy nhất là dấu hiệu: tích cực đại có thể đến từ hai số dương lớn nhất hoặc từ hai số nhỏ nhất (âm nhất). 

Vì vậy, chúng ta chỉ cần theo dõi bốn ứng viên: hai giá trị lớn nhất và hai giá trị nhỏ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mảng một lần trong khi vẫn duy trì hai giá trị lớn nhất và hai giá trị nhỏ nhất được thấy cho đến nay. 

Chúng tôi duy trì các thái cực này vì bất kỳ cặp tối ưu nào cũng phải xuất phát từ một trong các trường hợp biên này. 
2. Đối với mỗi phần tử$x$, cập nhật cặp tối đa$(max1, max2)$để có thể$max1 \ge max2$và tương tự cập nhật cặp tối thiểu$(min1, min2)$để có thể$min1 \le min2$. 

Điều này đảm bảo chúng tôi luôn giữ được những ứng viên tốt nhất cho những đóng góp tích cực và tiêu cực. 
3. Sau khi xử lý tất cả các phần tử, hãy tính hai câu trả lời ứng viên:$max1 \cdot max2$Và$min1 \cdot min2$. 
4. Trả về giá trị lớn nhất của hai giá trị này. 

Lý do chúng tôi chia thành hai trường hợp này là phép nhân hoạt động khác nhau khi đổi dấu. Các giá trị dương lớn sẽ trực tiếp tối đa hóa tích số, trong khi các giá trị âm có cường độ lớn có thể tạo ra tích dương lớn khi nhân với nhau. 

### Tại sao nó hoạt động 

Giải pháp tối ưu nào cũng chỉ phụ thuộc vào hai chỉ số được chọn$i < j$. Vì bất kỳ cặp nào cũng có thể được tạo liền kề sau khi xóa, nên vấn đề giảm xuống còn việc chọn hai phần tử để tối đa hóa tích của chúng. Cặp tốt nhất như vậy phải đến từ các giá trị cực trị: hai số lớn nhất hoặc hai số nhỏ nhất. Bất kỳ giá trị bên trong nào cũng không thể hoạt động tốt hơn các kết hợp này vì việc thay thế một phần tử đã chọn bằng một giá trị cực đoan hơn luôn cải thiện hoặc duy trì đóng góp tuyệt đối của sản phẩm trong trường hợp ghép nối chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        # initialize extremes
        max1 = max2 = -10**30
        min1 = min2 = 10**30
        
        for x in a:
            # update max pair
            if x > max1:
                max2 = max1
                max1 = x
            elif x > max2:
                max2 = x
            
            # update min pair
            if x < min1:
                min2 = min1
                min1 = x
            elif x < min2:
                min2 = x
        
        ans = max(max1 * max2, min1 * min2)
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai theo dõi hai giá trị lớn nhất và hai giá trị nhỏ nhất trong một lần thực hiện. Quá trình khởi tạo sử dụng các giá trị trọng điểm rất lớn để đảm bảo cập nhật chính xác ngay cả khi tất cả các số đều âm hoặc tất cả đều dương. 

Việc so sánh cuối cùng giữa hai sản phẩm ứng cử viên là cần thiết vì nó nắm bắt được cả sự thống trị của cặp tích cực và sự thống trị của cặp tiêu cực. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$[5, 0, 2, 1]$| Bước | tối đa1 | tối đa2 | phút1 | phút2 | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 5 | 5 | -inf | 5 | thông tin | khởi tạo | 
| 0 | 5 | 0 | 0 | 5 | cập nhật cả hai bên | 
| 2 | 5 | 2 | 0 | 5 | cập nhật bên tối đa | 
| 1 | 5 | 2 | 0 | 1 | cập nhật bên tối đa | 

Ứng viên là$5 \cdot 2 = 10$Và$0 \cdot 1 = 0$, vậy đáp án là$10$. 

Dấu vết này cho thấy kết quả tốt nhất đến từ hai số dương lớn nhất, không liên quan đến số 0 hoặc giá trị nhỏ hơn. 

### Ví dụ 2 

đầu vào:$[-8, 4, 3, 7, 1, -9]$| Bước | tối đa1 | tối đa2 | phút1 | phút2 | hành động | 
| --- | --- | --- | --- | --- | --- | 
| -8 | -8 | -inf | -8 | thông tin | ban đầu | 
| 4 | 4 | -8 | -8 | -8 | cập nhật cả hai | 
| 3 | 4 | 3 | -8 | -8 | cập nhật tối đa | 
| 7 | 7 | 4 | -8 | -8 | cập nhật tối đa | 
| 1 | 7 | 4 | -8 | -8 | không thay đổi | 
| -9 | 7 | 4 | -9 | -8 | cập nhật tối thiểu | 

Ứng viên là$7 \cdot 4 = 28$Và$(-9) \cdot (-8) = 72$, vậy đáp án là$72$. 

Điều này khẳng định tầm quan trọng của việc xem xét các cặp âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| vượt qua một lần cho mỗi trường hợp thử nghiệm | 
| Không gian |$O(1)$| chỉ có bốn biến được lưu trữ | 

Thuật toán xử lý từng phần tử một lần, phù hợp thoải mái trong tổng ràng buộc của$2 \cdot 10^5$các phần tử. Bộ nhớ không đổi đảm bảo không có chi phí vượt quá một vài biến số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    def input():
        return sys.stdin.readline()
    
    t = int(sys.stdin.readline())
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        
        max1 = max2 = -10**30
        min1 = min2 = 10**30
        
        for x in a:
            if x > max1:
                max2 = max1
                max1 = x
            elif x > max2:
                max2 = x
            
            if x < min1:
                min2 = min1
                min1 = x
            elif x < min2:
                min2 = x
        
        out.append(str(max(max1 * max2, min1 * min2)))
    
    return "\n".join(out)

# provided samples
assert run("""7
4
5 0 2 1
3
-1 1 0
5
2 0 -1 -4 0
6
-8 4 3 7 1 -9
6
0 3 -2 5 -4 -4
2
1000000000 910000000
7
-1 -7 -2 -5 -4 -6 -3
""") == """10
0
4
72
16
910000000000000000
42"""

# custom cases
assert run("""1
2
1 2
""") == "2", "simple positive pair"

assert run("""1
2
-5 -6
""") == "30", "two negatives"

assert run("""1
3
-10 1 2
""") == "2", "skip negative extreme"

assert run("""1
4
0 -1 -2 -3
""") == "6", "best negative pair"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 1 2 | 2 | trường hợp tích cực cơ bản | 
| -5 -6 | 30 | sự thống trị của cặp âm | 
| -10 1 2 | 2 | phớt lờ những thái cực có hại | 
| 0 -1 -2 -3 | 6 | tương tác bằng không và tiêu cực | 

## Vỏ cạnh 

Một tình huống phức tạp xảy ra khi mảng chứa cả giá trị dương lớn và âm có cường độ lớn. Ví dụ,$[-100, 1, 2, 3, 4]$. Thuật toán theo dõi cả hai thái cực, do đó cặp tối đa trở thành$4 \cdot 3 = 12$, trong khi cặp tối thiểu không có tính cạnh tranh vì chỉ có một âm lớn. 

Vì$[-5, -6, 2]$, các giá trị được theo dõi kết thúc là max1 = 2, max2 = -5 và min1 = -6, min2 = -5. Thuật toán đánh giá$2 \cdot -5 = -10$Và$(-6) \cdot (-5) = 30$, chọn đúng cặp âm. 

Trong trường hợp có số 0 như$[0, -1, -2]$, các số 0 đương nhiên được đưa vào theo dõi tối đa nhưng không ảnh hưởng đến cặp tối thiểu, đảm bảo rằng câu trả lời đúng đến từ$(-1) \cdot (-2)$.
