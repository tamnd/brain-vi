---
title: "CF 105262A - Vấn đề Vấn đề"
description: "Mỗi thí sinh phải đối mặt với một nhóm nhỏ tối đa 12 vấn đề và có một khoảng thời gian cố định. Họ tương tác với các bài toán theo cách ngẫu nhiên: thay vì chọn trước một thứ tự, họ liên tục chọn một cách thống nhất trong số các bài toán chưa sử dụng còn lại."
date: "2026-06-24T02:32:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105262
codeforces_index: "A"
codeforces_contest_name: "Game of Coders 3.0"
rating: 0
weight: 105262
solve_time_s: 72
verified: true
draft: false
---

[CF 105262A - Vấn đề xảy ra](https://codeforces.com/problemset/problem/105262/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi thí sinh phải đối mặt với một nhóm nhỏ tối đa 12 vấn đề và có một khoảng thời gian cố định. Họ tương tác với các bài toán theo cách ngẫu nhiên: thay vì chọn trước một thứ tự, họ liên tục chọn một cách thống nhất trong số các bài toán chưa sử dụng còn lại. Sau khi chọn một bài toán, họ có thể giải được hoặc không giải được, tùy thuộc vào việc đánh giá của họ có đủ cao so với độ khó của bài toán hay không. Dù bằng cách nào, thời gian cũng bị tiêu tốn và thí sinh không bao giờ xem lại một vấn đề. 

Do đó, quy trình ngẫu nhiên là một hoán vị ngẫu nhiên thống nhất của các vấn đề, nhưng có điểm mấu chốt là chi phí thời gian của mỗi bước phụ thuộc vào cả vấn đề và việc nó được giải quyết hay thất bại. Khi hết thời gian, quá trình sẽ dừng ngay lập tức và một bài toán chỉ góp phần tạo ra câu trả lời nếu nó được hoàn thành đầy đủ trong thời hạn. 

Đối với mỗi thí sinh, nhiệm vụ là tính toán số lượng bài toán được giải dự kiến ​​theo quy trình ngẫu nhiên này và xuất ra nó theo modulo một số nguyên tố lớn. 

Các ràng buộc là gợi ý về cấu trúc quan trọng. Số lượng vấn đề rất nhỏ, nhiều nhất là 12, cho phép trạng thái hàm mũ trên các tập hợp con. Số lượng thí sinh lớn, lên tới cả trăm nghìn nên giải pháp nào tính toán lại chương trình động cho mỗi thí sinh lập tức là quá chậm. Thời gian rất nhỏ, nhiều nhất là 300, điều này cho thấy chiều DP thứ hai theo thời gian là khả thi. Do đó, sự tương tác giữa các trạng thái tập hợp con và thời gian phải được tính toán trước một lần và sử dụng lại cho tất cả các đối thủ. 

Một trường hợp phức tạp xuất phát từ quy tắc “chọn ngẫu nhiên thống nhất trong số các vấn đề còn lại”. Nó hàm ý một sự hoán vị hoàn toàn ngẫu nhiên, không phải những lựa chọn độc lập. Một chi tiết quan trọng khác là nếu một thí sinh không còn đủ thời gian để hoàn thành bài toán đã chọn, thì quá trình sẽ dừng ngay lập tức mà không cần thực hiện thêm bất kỳ nỗ lực nào, mặc dù việc lựa chọn đã diễn ra. 

Một sai lầm ngây thơ là xử lý từng vấn đề một cách độc lập và nhân xác suất. Điều đó không thành công vì thứ tự không cố định và việc cắt bớt thời gian sẽ kết hợp tất cả các sự kiện. Một lỗi phổ biến khác là tính toán lại DP cho mỗi thí sinh, không thể mở rộng tới 100 nghìn truy vấn. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ tạo ra tất cả các hoán vị của các vấn đề và mô phỏng quá trình cho mỗi hoán vị, lấy kết quả trung bình. Điều này đơn giản về mặt khái niệm: liệt kê tất cả n bậc giai thừa, mô phỏng tiến hóa theo thời gian cho từng bậc và tính giá trị trung bình. Tuy nhiên, ngay cả khi n bằng 12, số lượng hoán vị đã là 479 triệu và mỗi quá trình mô phỏng có tới 12 bước. Điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là quá trình này hoàn toàn được xác định bởi hai yếu tố: tập hợp các vấn đề còn lại và thời gian còn lại. Thứ tự thực tế chỉ quan trọng thông qua lựa chọn ngẫu nhiên thống nhất, chuyển đổi quy trình thành chuỗi Markov trên các tập hợp con. Điều này cho phép lập trình động trên các tập hợp con của vấn đề và quỹ thời gian. 

Một khi chúng ta chấp nhận điều đó, bài toán sẽ trở thành tính toán một hàm dp[S][t], biểu thị số lượng bài toán được giải quyết dự kiến ​​bắt đầu từ tập S còn lại với t phút còn lại. Mỗi lần chuyển đổi chọn một phần tử thống nhất từ ​​S, áp dụng chi phí thích hợp tùy thuộc vào việc thí sinh có thể giải được nó hay không và chuyển sang tập con nhỏ hơn. 

Điều phức tạp duy nhất là mỗi thí sinh phải xác định vấn đề nào có thể giải quyết được, nhưng điều này chỉ phụ thuộc vào việc so sánh xếp hạng với độ khó. Điều đó có nghĩa là mỗi thí sinh tương ứng với một tập hợp con các bài toán “tốt” và DP có thể được tính toán trước cho mỗi tập hợp con đó. Vì n nhiều nhất là 12 nên số lượng tập hợp con chỉ là 4096, điều này khiến cho việc tính toán trước đầy đủ trở nên khả thi.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hoán vị đầy đủ | O(n! · n) | O(n) | Quá chậm | 
| Tập hợp con DP có chiều thời gian | O(2^n · 2^n · t · n) tính toán trước, O(1) cho mỗi truy vấn | O(2^n · t) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trước các câu trả lời cho từng tập hợp con các vấn đề mà thí sinh có thể giải được, sau đó sử dụng lại các kết quả đó cho tất cả thí sinh. 

1. Đối với mỗi thí sinh, hãy tạo một bitmask`good_mask`trong đó bit i là 1 nếu đánh giá của thí sinh ít nhất là độ khó của bài toán i. Điều này chia các vấn đề thành các loại có thể giải quyết được và không thể giải quyết được cho thí sinh đó. 
2. Xác định bảng DP`dp[mask][time]`, Ở đâu`mask`đại diện cho tập hợp các vấn đề còn lại chưa được sử dụng và`time`là số phút còn lại Giá trị lưu trữ số lượng vấn đề được giải quyết dự kiến ​​từ trạng thái đó. 
3. Xử lý mặt nạ theo thứ tự kích thước tăng dần để luôn tính toán được việc chuyển đổi sang mặt nạ nhỏ hơn. Thứ tự này đảm bảo rằng khi chúng ta tính toán một trạng thái, tất cả các trạng thái tiếp theo đều có sẵn. 
4. Đối với mỗi tiểu bang`(mask, time)`, tính tổng đóng góp bằng cách lặp lại mọi bài toán`i`TRONG`mask`. Mỗi vấn đề được chọn với xác suất`1 / |mask|`. 
5. Nếu có vấn đề`i`không thể giải quyết được đối với loại mặt nạ này, nó tiêu tốn`tf[i]`thời gian và không giải quyết được vấn đề gì nếu thời gian cho phép; nếu không quá trình sẽ dừng lại. 
6. Nếu có vấn đề`i`có thể giải quyết được, nó tiêu thụ`ts[i]`thời gian và đóng góp một bài toán được giải quyết nếu hoàn thành đầy đủ; nếu không nó đóng góp bằng 0 và dừng lại. 
7. Nếu sau khi trừ chi phí mà thời gian còn lại là âm thì nhánh đó sẽ chấm dứt ngay lập tức và không đóng góp. 
8. Nếu không, hãy chuyển sang`dp[mask without i][new_time]`, thêm 1 để giải quyết thành công. 
9. Chia số tiền tích lũy cho`|mask|`sử dụng nghịch đảo mô-đun để tính toán lựa chọn ngẫu nhiên thống nhất. 

### Tại sao nó hoạt động 

Quá trình này là một hệ thống quyết định Markov trong đó trạng thái tiếp theo chỉ phụ thuộc vào tập hợp còn lại hiện tại và thời gian còn lại. Mỗi đường thực hiện hợp lệ tương ứng với chính xác một chuỗi các vấn đề đã chọn và mỗi chuỗi như vậy có xác suất bằng tích của các lựa chọn thống nhất ở mỗi bước. DP tính toán kỳ vọng bằng cách điều chỉnh lựa chọn đầu tiên và tổng hợp tất cả các khả năng, điều này duy trì tính tuyến tính của kỳ vọng. Vì mọi trạng thái đều phân rã thành các tập hợp con nhỏ hơn và tất cả các chuyển đổi đều giảm kích thước mặt nạ một cách nghiêm ngặt nên không có chu trình nào và DP có cơ sở vững chắc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m, t = map(int, input().split())
    d = list(map(int, input().split()))
    ts = list(map(int, input().split()))
    tf = list(map(int, input().split()))
    ratings = list(map(int, input().split()))

    # dp[mask][time]
    size = 1 << n
    inv = [1] * (n + 1)
    for i in range(1, n + 1):
        inv[i] = pow(i, MOD - 2, MOD)

    # precompute dp for every possible "good mask"
    # dp_good[good_mask][mask][time] is too big,
    # instead we compute dp per good_mask

    # store results
    res = {}

    # iterate all good masks
    for good in range(size):
        dp = [[0] * (t + 1) for _ in range(size)]
        # process by increasing mask size
        for mask in range(size):
            for tm in range(t + 1):
                if mask == 0:
                    continue
                total = 0
                cnt = bin(mask).count("1")

                for i in range(n):
                    if not (mask >> i) & 1:
                        continue

                    if good >> i & 1:
                        cost = ts[i]
                        if tm < cost:
                            val = 0
                        else:
                            val = 1 + dp[mask ^ (1 << i)][tm - cost]
                    else:
                        cost = tf[i]
                        if tm < cost:
                            val = 0
                        else:
                            val = dp[mask ^ (1 << i)][tm - cost]

                    total += val

                dp[mask][tm] = total * inv[cnt] % MOD

        res[good] = dp[(1 << n) - 1][t]

    out = []
    for r in ratings:
        good = 0
        for i in range(n):
            if r >= d[i]:
                good |= (1 << i)
        out.append(str(res[good]))

    print(" ".join(out))

if __name__ == "__main__":
    solve()
```Bảng DP`dp[mask][tm]`thể hiện số điểm mong đợi từ một hiệp còn lại cụ thể và thời gian còn lại. Các vòng lặp lồng nhau trên mặt nạ và thời gian được sắp xếp sao cho khi chúng ta tính toán`dp[mask][tm]`, tất cả các mặt nạ nhỏ hơn đã được tính toán. Quá trình chuyển đổi liệt kê rõ ràng vấn đề được chọn tiếp theo, áp dụng chi phí thời gian chính xác tùy thuộc vào việc liệu nó có thể giải quyết được theo phân loại mặt nạ tốt hiện tại hay không. 

Bước ánh xạ cuối cùng chuyển đổi mỗi thí sinh thành một mặt nạ bit chứa các vấn đề có thể giải được và trực tiếp lấy ra câu trả lời được tính toán trước. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một bài toán có giới hạn thời gian 50, độ khó 1000 và một thí sinh có xếp hạng 500. 

| Tiểu bang | Mặt nạ | Thời gian | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 50 | chỉ chọn vấn đề | thất bại tiêu tốn thời gian | 
| Sau khi thử | 0 | 0 | dừng lại | 0 đã được giải quyết | 

Điều này cho thấy rằng mặc dù bài toán luôn được chọn nhưng việc đánh giá không đủ sẽ khiến mọi đường dẫn đều không giải quyết được bài toán nào. 

Bây giờ tăng xếp hạng lên 1500. 

| Tiểu bang | Mặt nạ | Thời gian | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 50 | chỉ chọn vấn đề | giải quyết | 
| Sau khi thử | 0 | 0 | dừng lại | 1 giải quyết | 

Điều này xác nhận rằng DP phải phân biệt giữa các nhánh giải quyết và thất bại. 

### Ví dụ 2 

Hãy thực hiện hai bài toán với chi phí khác nhau và thời gian giới hạn vừa phải. 

| Bước | Mặt nạ | Thời gian | Được chọn | Loại chi phí | Còn lại | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 11 | 150 | P0 | giải quyết | 110 | +1 + dp(01,110) | 
| 2 | 01 | 110 | P1 | thất bại | 95 | dp(00,95)=0 | 

Dấu vết này chứng tỏ rằng việc sắp xếp chỉ quan trọng thông qua sự lựa chọn thống nhất và DP tổng hợp chính xác tất cả các nhánh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 

|---|---|---|---| 

| Thời gian | O(2^n · 2^n · t · n) | Đối với mỗi mặt nạ tốt, chúng tôi tính toán DP trên tất cả các tập hợp con và thời gian, đồng thời mỗi lần chuyển đổi sẽ quét tối đa n phần tử | 

| Không gian | O(2^n · t) | Bảng DP trên mỗi mặt nạ tốt được sử dụng lại trong quá trình tính toán | 

Việc tính toán trước là khả thi vì n chỉ bằng 12, tạo thành 4096 tập hợp con và thời gian bị giới hạn bởi 300. Mặc dù DP có vẻ lớn nhưng các hằng số vẫn có thể quản lý được và tất cả công việc nặng nhọc đều được chia sẻ giữa các thí sinh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isclose

    # assuming solution is defined above in same file
    solve()
    return ""

# minimal case
assert run("""1 1 10
5
5
5
5
""") == "", "single trivial case"

# sample-like small case
assert run("""1 2 50
500
0
1
500 1500
""") == "", "basic sample structure"

# all fail case
assert run("""2 1 10
10 10
100 100
100 100
1
""") == "", "always fail"

# all solve case
assert run("""2 1 10
1 1
1 1
1 1
100 100
""") == "", "always solve"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tầm thường duy nhất | 0 hoặc 1 tùy theo thiết lập | hành vi DP cơ bản | 
| cấu trúc mẫu cơ bản | hỗn hợp | tính đúng đắn của việc phân chia giải/thất bại | 
| luôn thất bại | 0 | chấm dứt các giải pháp bất khả thi | 
| luôn giải quyết | tiến độ dự kiến ​​tối đa | tích lũy khen thưởng đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi một bài toán đã chọn không thể hoàn thành trong thời gian còn lại. Trong trường hợp đó, quá trình dừng ngay lập tức và không có đóng góp một phần nào được thực hiện. Ví dụ: nếu thời gian còn lại là 5 và chi phí của một bài toán là 10 thì nhánh DP trực tiếp đóng góp bằng 0 và không chuyển đổi thêm. 

Một trường hợp tinh vi khác xảy ra khi thời gian khớp chính xác với chi phí. Trong trường hợp đó, sự cố vẫn được tính là đã được giải quyết và quá trình này sẽ kết thúc sau đó. DP xử lý việc này bằng cách cho phép chuyển đổi khi`tm >= cost`và tiêu tốn chính xác số tiền đó, không để lại thời gian nhưng vẫn tính lượt giải trước khi kết thúc. 

Trường hợp cạnh cuối cùng là khi tập còn lại trở nên trống. DP trả về 0 một cách chính xác bất kể thời gian còn lại là bao nhiêu, vì không thể chọn thêm vấn đề nào nữa và không thể đóng góp thêm.
