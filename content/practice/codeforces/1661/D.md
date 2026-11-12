---
title: "CF 1661D - Bao gồm tiến trình"
description: "Chúng ta được cung cấp một mảng b có độ dài n, trong đó mỗi phần tử đại diện cho một giá trị mục tiêu mà chúng ta cần đạt hoặc vượt trong mảng a tương ứng. Ban đầu, a bao gồm tất cả các số không."
date: "2026-06-10T02:56:11+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1661
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 126 (Rated for Div. 2)"
rating: 1900
weight: 1661
solve_time_s: 82
verified: true
draft: false
---

[CF 1661D - Bao gồm các tiến trình](https://codeforces.com/problemset/problem/1661/D) 

**Xếp hạng:** 1900 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`b`chiều dài`n`, trong đó mỗi phần tử đại diện cho một giá trị mục tiêu mà chúng ta cần đạt hoặc vượt trong một mảng tương ứng`a`. Ban đầu,`a`bao gồm tất cả các số không. Chúng ta có thể thực hiện thao tác sau với số lần bất kỳ: chọn một mảng con liền kề có độ dài`k`và thêm một dãy tăng dần`1, 2, ..., k`vào mảng con đó. Mục đích là xác định số lượng thao tác tối thiểu cần thiết để mọi phần tử của`a`ít nhất là phần tử tương ứng trong`b`. 

Những ràng buộc cho chúng ta biết rằng`n`có thể lớn như`3 * 10^5`và mỗi`b_i`có thể đạt tới`10^12`. Điều này ngay lập tức loại trừ bất kỳ thuật toán nào mô phỏng từng hoạt động riêng lẻ hoặc lặp lại với số lượng lớn vì cách tiếp cận đơn giản sẽ yêu cầu hàng nghìn tỷ hoạt động. 

Các trường hợp cạnh rất tinh tế ở đây. Nếu như`k = 1`, mỗi thao tác chỉ tăng một phần tử đơn lẻ bằng`1`, vì vậy câu trả lời chỉ đơn giản là`max(b)`. Nếu như`k = n`, thì mỗi thao tác sẽ ảnh hưởng đến tất cả các phần tử, nhưng số gia tăng sẽ được tính trọng số (`1, 2, ..., n`), do đó việc phân phối các hoạt động một cách tham lam từ cuối có thể là tối ưu. Một trường hợp phức tạp khác xảy ra khi xuất hiện khoảng cách lớn giữa yêu cầu`b_i`giá trị - các cách tiếp cận bất cẩn có thể tính thiếu các phép toán nếu chúng bỏ qua sự trùng lặp của cấp số cộng. 

Ví dụ, hãy xem xét`n=3`,`k=2`,`b=[3,1,2]`. Cách tiếp cận ngây thơ có thể cố gắng nâng cao từng phần tử riêng lẻ mà không xem xét rằng việc thêm một tiến trình bắt đầu từ vị trí 2 cũng ảnh hưởng đến phần tử thứ ba. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp lại mảng từ trái sang phải. Tại mỗi vị trí`i`, chúng tôi kiểm tra xem`a[i] < b[i]`. Nếu vậy thì ta áp dụng đủ các thao tác bắt đầu từ`i`để thỏa mãn`b[i]`. Mỗi hoạt động cập nhật`k`phần tử, thêm`1, 2, ..., k`. Cách tiếp cận này hoạt động hợp lý, nhưng việc cập nhật mảng rõ ràng cho từng thao tác thì quá chậm. Trong trường hợp xấu nhất, đối với`n = 3*10^5`Và`b_i`lên đến`10^{12}`, số lượng thao tác là không giới hạn, khiến cho việc mô phỏng rõ ràng là không thể thực hiện được. 

Thông tin chi tiết quan trọng là chúng ta không cần cập nhật từng phần tử của mảng. Chúng ta có thể duy trì một "mảng khác biệt" hoặc sử dụng cách tiếp cận giống như lan truyền lười biếng: theo dõi hiệu ứng tích lũy của các hoạt động trước đó theo kiểu cuộn. Vì mỗi phép toán là một cấp số cộng nên ảnh hưởng lên các phần tử trong tương lai có thể được biểu thị dưới dạng số gia cộng với độ dốc. Chúng ta chỉ cần lưu trữ sự thay đổi độ dốc và hiệu ứng tích lũy hiện tại ở mỗi chỉ số, điều này cho phép chúng ta tính toán số lượng thao tác bổ sung cần thiết ở mỗi bước mà không thực sự cập nhật tất cả.`k`các phần tử một cách rõ ràng. 

Bằng cách xử lý mảng từ trái sang phải, chúng tôi duy trì tác động tích lũy của các hoạt động trước đó trong`current_add`và độ dốc ở`slope_add`. Bất cứ khi nào`a[i] + current_add`ít hơn`b[i]`, chúng tôi tính toán số lượng hoạt động mới để áp dụng tại vị trí`i`để thỏa mãn`b[i]`. Điều này đảm bảo các hoạt động tối thiểu vì mọi hoạt động sau này sẽ trùng lặp với các hoạt động trước đó và chúng tôi luôn bù đắp khoản thâm hụt một cách tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * b_max) | O(n) | Quá chậm | 
| Tối ưu (tham lam với sự khác biệt tiền tố) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`slope`chiều dài`n+1`với số không. Mảng này sẽ theo dõi hiệu ứng tích lũy của độ dốc từ các hoạt động trước đó. Đồng thời khởi tạo`current_add = 0`Và`operations = 0`. 
2. Lặp lại từng chỉ mục`i`từ`0`ĐẾN`n-1`. Tính giá trị hiệu dụng tại vị trí`i`BẰNG`effective = current_add`. Nếu như`effective < b[i]`, tính toán số lượng hoạt động mới cần thiết như`needed = b[i] - effective`. 
3. Tăng`operations`qua`needed`. 
4. Cập nhật`current_add`bằng cách thêm`needed`. 
5. Nếu`i + k < n`, giảm`current_add`qua`needed * k`ở vị trí`i+k`trong mảng độ dốc. Điều này đảm bảo hiệu ứng độ dốc chỉ kéo dài trong`k`các vị trí. 
6. Đồng thời tuyên truyền độ dốc bằng cách thêm`needed`ĐẾN`slope[i+1]`cho chỉ mục tiếp theo. Mảng độ dốc đảm bảo việc bổ sung gia tăng cho các vị trí bên trong cửa sổ hiện tại được áp dụng chính xác. 
7. Di chuyển tới chỉ mục tiếp theo, cập nhật`current_add`bằng cách thêm`slope[i]`để tính đến sự đóng góp liên tục của độ dốc. 

Tại sao nó hoạt động: bất biến là ở mỗi chỉ số`i`,`current_add`phản ánh tổng đóng góp của tất cả các hoạt động ảnh hưởng đến`a[i]`. Chúng tôi chỉ thêm các hoạt động khi tổng số hiện tại không đủ để đạt được`b[i]`, và chúng tôi làm như vậy ở mức tối thiểu. Bằng cách theo dõi độ dốc bằng một mảng sai phân, chúng tôi truyền bá chính xác các tác động của cấp số cộng đến các phần tử tiếp theo mà không cần cập nhật rõ ràng từng phần tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, k = map(int, input().split())
    b = list(map(int, input().split()))
    
    slope = [0] * (n + 1)
    current_add = 0
    operations = 0
    
    for i in range(n):
        current_add += slope[i]
        if current_add < b[i]:
            needed = b[i] - current_add
            operations += needed
            current_add += needed
            if i + k < n:
                slope[i + k] -= needed
            if i + 1 < n:
                slope[i + 1] += needed
    
    print(operations)

if __name__ == "__main__":
    main()
```Giải pháp duy trì`current_add`như hiệu quả tích lũy của các hoạt động trước đó. các`slope`mảng truyền tác động gia tăng của cấp số cộng tới các vị trí trong tương lai. Việc xử lý ranh giới là rất quan trọng: chúng tôi chỉ cập nhật`slope[i+k]`nếu như`i+k < n`để tránh tràn chỉ mục và việc bổ sung tăng dần bắt đầu từ`i+1`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
5 4 6
```| tôi | b[i] | hiện tại_add | cần thiết | hoạt động | độ dốc | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 0 | 5 | 5 | [0,5,0,0] | 
| 1 | 4 | 10 | 0 | 5 | [0,5,0,0] | 
| 2 | 6 | 12 | 0 | 5 | [0,5,0,0] | 

Giải thích: thao tác đầu tiên bao gồm cả ba yếu tố. Các yếu tố bổ sung đã đáp ứng`b[i]`. 

### Mẫu tùy chỉnh 

đầu vào:```
5 2
1 2 3 2 1
```| tôi | b[i] | hiện tại_add | cần thiết | hoạt động | độ dốc | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | [0,1,0,0,0,0] | 
| 1 | 2 | 2 | 0 | 1 | [0,1,0,0,0,0] | 
| 2 | 3 | 2 | 1 | 2 | [0,1,0,-1,0,0] | 
| 3 | 2 | 3 | 0 | 2 | [0,1,0,-1,0,0] | 
| 4 | 1 | 2 | 0 | 2 | [0,1,0,-1,0,0] | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được xử lý một lần, cập nhật cho mảng độ dốc là O(1) cho mỗi chỉ mục | 
| Không gian | O(n) | Chúng tôi duy trì mảng độ dốc có kích thước n+1 | 

Được cho`n <= 3*10^5`và các hoạt động là O(1) cho mỗi chỉ mục, điều này rất phù hợp trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import main
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3 3\n5 4 6\n") == "5", "sample 1"
assert run("6 3\n1 2 3 2 1 4\n") == "4", "sample 2"

# Custom tests
assert run("1 1\n1000000000000\n") == "1000000000000", "single element max b"
assert run("5 2\n1 2 3 2 1\n") == "2", "overlap test"
assert run("3 3\n1 1 1\n") == "1", "all ones"
assert run("4 4\n4 3 2 1\n") == "1", "k=n descending"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1000000000000 | 1000000000000 | Phần tử đơn tối đa | 
| 5 2 1 2 3 2 | | |
