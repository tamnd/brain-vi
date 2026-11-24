---
title: "CF 920A - Tưới Vườn"
description: "Chúng ta có một khu vườn tuyến tính với n luống liên tiếp và một tập hợp con các luống chứa vòi nước. Mỗi vòi, sau khi được bật, sẽ tưới nước cho luống mà nó chiếm giữ ngay lập tức và trong mỗi giây tiếp theo, nó sẽ mở rộng phạm vi bao phủ thêm một luống theo cả hai hướng."
date: "2026-06-12T09:46:32+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 920
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 37 (Rated for Div. 2)"
rating: 1000
weight: 920
solve_time_s: 128
verified: true
draft: false
---

[CF 920A - Tưới vườn](https://codeforces.com/problemset/problem/920/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một khu vườn tuyến tính với _n_ luống liên tiếp và một tập hợp con các luống chứa vòi nước. Mỗi vòi, sau khi được bật, sẽ tưới nước cho luống mà nó chiếm giữ ngay lập tức và trong mỗi giây tiếp theo, nó sẽ mở rộng phạm vi bao phủ thêm một luống theo cả hai hướng. Mục tiêu của chúng tôi là xác định số giây tối thiểu cần thiết để tất cả các luống được tưới nếu tất cả các vòi được bật đồng thời. 

Đầu vào chỉ định nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp số giường _n_, số lượng vòi _k_ và vị trí của vòi. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất: giây sớm nhất khi mỗi luống đều được tưới nước. 

Các ràng buộc nhỏ: _n_ nhiều nhất là 200 và tổng _n_ trên tất cả các trường hợp thử nghiệm không vượt quá 200. Điều này có nghĩa là bất kỳ giải pháp nào có giá trị khoảng O(n²) cho mỗi trường hợp thử nghiệm sẽ chạy thoải mái, vì vậy chúng tôi có đủ khả năng mô phỏng hoặc xử lý từng giường riêng lẻ nếu cần. 

Các trường hợp cạnh không rõ ràng bao gồm các khu vườn nơi các vòi được tập trung ở một đầu hoặc khi nhiều vòi chồng lên nhau. Ví dụ: nếu _n = 5_ và vòi ở vị trí 1 và 2, luống ngoài cùng bên trái sẽ được tưới ngay lập tức, nhưng luống ngoài cùng bên phải lại ở xa hơn và cách tiếp cận ngây thơ chỉ nhìn vào vòi gần nhất có thể đánh giá thấp thời gian cần thiết. Một kịch bản tinh vi khác xảy ra khi chỉ có một vòi nước ở đầu vườn; thuật toán phải tính đến khoảng cách xa nhất từ ​​​​bất kỳ vòi nào đến giường, không chỉ gần nhất. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ là mô phỏng quá trình tưới nước từng giây một. Chúng tôi có thể duy trì một mảng đại diện cho các luống trong vườn và trong mỗi giây, hãy mở rộng phạm vi tưới nước của mỗi vòi. Sau mỗi giây, chúng tôi kiểm tra xem tất cả các luống đã được tưới nước chưa. Điều này đúng nhưng không hiệu quả: đối với khu vườn trong trường hợp xấu nhất có 200 luống và một vòi duy nhất, mô phỏng này có thể thực hiện tới 200 thao tác mỗi giây trong 200 giây, dẫn đến khoảng 40.000 thao tác. Mặc dù có thể chấp nhận được với các giới hạn của vấn đề, nhưng nó dài dòng không cần thiết. 

Cách tiếp cận tối ưu bắt nguồn từ việc quan sát rằng đối với bất kỳ luống nào, thời gian tưới tối thiểu được xác định bởi vòi gần nhất. Nếu chúng ta sắp xếp các vòi và kiểm tra khoảng cách từ mỗi giường đến vòi gần nhất thì khoảng cách tối đa tối thiểu trên tất cả các giường sẽ cho câu trả lời. Điều này hiệu quả vì nước lan truyền đối xứng từ mỗi vòi với tốc độ một giường mỗi giây, do đó, giường xa nhất so với tất cả các vòi sẽ quyết định tổng thời gian cần thiết. 

Brute-force hoạt động vì nó mô phỏng quá trình theo đúng nghĩa đen, nhưng nó không đạt được sự tinh tế và rõ ràng. Nhận xét rằng thời gian cần thiết chỉ đơn giản là khoảng cách tối đa từ giường đến vòi nước gần nhất cho phép chúng tôi giảm vấn đề xuống còn quét tuyến tính đơn giản sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Được chấp nhận nhưng dài dòng | 
| Tối ưu | O(n log n) | O(n) | Sạch sẽ, được chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc số giường _n_, số lượng vòi _k_ và danh sách các vị trí vòi. Sắp xếp các vị trí nhấn giúp đơn giản hóa việc tính toán khoảng cách. 
2. Khởi tạo một biến để theo dõi thời gian tối đa cần thiết để tưới nước cho luống. Điều này tượng trưng cho câu trả lời. 
3. Lặp lại trên mỗi ô từ 1 đến _n_. Đối với mỗi giường, hãy tính khoảng cách tuyệt đối đến từng vòi, sau đó chọn khoảng cách tối thiểu. Điều này mang lại thời gian cần thiết để tưới nước cho chiếc giường cụ thể này. 
4. Cập nhật thời gian tối đa nếu khoảng cách tối thiểu của giường hiện tại lớn hơn khoảng cách tối đa trước đó. Sau khi quét tất cả các luống, thời gian tối đa là giây sớm nhất khi toàn bộ khu vườn được tưới nước. 
5. In thời gian tối đa cho test case hiện tại.

Tại sao nó hoạt động: Thuật toán đảm bảo rằng mọi giường đều được tính đến. Nước từ nhiều vòi không làm giảm thời gian tưới cho luống dưới khoảng cách từ vòi gần nhất, luống xa nhất sẽ quyết định tổng thời gian tưới. Việc sắp xếp là tùy chọn đối với kích thước đầu vào nhỏ này, nhưng nó làm cho việc tính toán lần nhấn gần nhất trở nên đơn giản hơn về mặt khái niệm. Điều bất biến là ở mỗi lần lặp,`max_time`giữ khoảng cách tối thiểu dài nhất gặp phải cho đến nay. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    taps = list(map(int, input().split()))
    max_time = 0
    for bed in range(1, n + 1):
        min_dist = min(abs(bed - tap) for tap in taps)
        max_time = max(max_time, min_dist)
    print(max_time + 1)
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó sẽ đọc kích thước khu vườn và vị trí vòi. Vòng lặp trên giường tính toán khoảng cách đến từng vòi, chọn vòi gần nhất và theo dõi khoảng cách tối đa. Chúng tôi thêm một vì giây đầu tiên tưới nước cho giường của vòi. Việc xử lý cẩn thận các chỉ số đảm bảo chúng tôi không đi quá giới hạn và tôn trọng việc đánh số giường dựa trên 1. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
5 1
3
```| Giường | Khoảng cách đến vòi | Khoảng cách tối thiểu | Thời gian tối đa cho đến nay | 
| --- | --- | --- | --- | 
| 1 | [2] | 2 | 2 | 
| 2 | [1] | 1 | 2 | 
| 3 | [0] | 0 | 2 | 
| 4 | [1] | 1 | 2 | 
| 5 | [2] | 2 | 2 | 

Khoảng cách tối thiểu tối đa là 2, vì vậy tổng số giây cần thiết là 3. Điều này phù hợp với kết quả đầu ra dự kiến. 

Đối với đầu vào:```
3 3
1 2 3
```| Giường | Khoảng cách đến vòi | Khoảng cách tối thiểu | Thời gian tối đa cho đến nay | 
| --- | --- | --- | --- | 
| 1 | [0,1,2] | 0 | 0 | 
| 2 | [1,0,1] | 0 | 0 | 
| 3 | [2,1,0] | 0 | 0 | 

Khoảng cách tối thiểu tối đa là 0, tổng số giây cần thiết là 1. 

Những dấu vết này chứng tỏ rằng thuật toán xác định chính xác chiếc giường xa nhất so với bất kỳ vòi nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Đối với mỗi giường, chúng tôi tính toán khoảng cách tới tất cả k vòi. Với n 200 và k ≤ n thì điều này chấp nhận được. | 
| Không gian | O(n + k) | Mảng cho các lần nhấn và theo dõi vô hướng đơn giản về thời gian tối đa. | 

Cho n và k nhỏ và có tổng trên tất cả các trường hợp thử nghiệm ≤ 200, giải pháp này chạy tốt trong giới hạn 1 giây và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution function inline
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        taps = list(map(int, input().split()))
        max_time = 0
        for bed in range(1, n + 1):
            min_dist = min(abs(bed - tap) for tap in taps)
            max_time = max(max_time, min_dist)
        print(max_time + 1)
    return output.getvalue().strip()

# provided samples
assert run("3\n5 1\n3\n3 3\n1 2 3\n4 1\n1") == "3\n1\n4", "sample 1"

# custom cases
assert run("1\n1 1\n1") == "1", "single bed with tap"
assert run("1\n5 2\n1 5") == "3", "taps at ends"
assert run("1\n6 1\n3") == "4", "single tap in middle"
assert run("1\n6 3\n1 3 6") == "2", "multiple taps uneven"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | giường đơn có vòi | 
| 5 2 1 5 | 3 | vòi ở hai đầu đối diện, phủ giường xa nhất | 
| 6 1 3 | 4 | chạm một lần vào giữa, tính toán khoảng cách xa nhất | 
| 6 3 1 3 6 | 2 | nhiều lần nhấn, đảm bảo thuật toán chọn lần nhấn gần nhất | 

## Vỏ cạnh 

Đối với khu vườn có một luống có vòi ở luống đó, hãy nhập`1 1\n1`, thuật toán tính khoảng cách tối thiểu 0 cho giường 1, cộng thêm 1, cho tổng số giây là 1. Đối với một khu vườn nơi các vòi chỉ chiếm các đầu,`5 2\n1 5`, giường 2 và 4 có khoảng cách tối thiểu là 1 đến vòi gần nhất, giường 3 có khoảng cách 2. Khoảng cách tối thiểu tối đa là 2, tổng số giây là 3. Các ví dụ này xác nhận cách tiếp cận xử lý ranh giới và phạm vi một lần nhấn một cách chính xác.
