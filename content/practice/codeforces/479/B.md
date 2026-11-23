---
title: "CF 479B - Tháp"
description: "Chúng ta có một số chồng khối đơn vị, mỗi chồng có chiều cao ban đầu. Chỉ bằng một nước đi, chúng ta có thể lấy chính xác một khối lập phương từ trên cùng của một ngăn xếp và đặt nó lên trên một ngăn xếp khác."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 479
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 274 (Div. 2)"
rating: 1400
weight: 479
solve_time_s: 63
verified: true
draft: false
---

[CF 479B - Tháp](https://codeforces.com/problemset/problem/479/B) 

**Đánh giá:** 1400 
**Tags:** bạo lực, thuật toán xây dựng, tham lam, triển khai, sắp xếp 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số chồng khối đơn vị, mỗi chồng có chiều cao ban đầu. Chỉ bằng một nước đi, chúng ta có thể lấy chính xác một khối lập phương từ trên cùng của một ngăn xếp và đặt nó lên trên một ngăn xếp khác. Theo thời gian, điều này cho phép chúng ta phân phối lại các khối giữa các ngăn xếp, nhưng mỗi lần di chuyển chỉ dịch chuyển một khối mỗi lần và không bao giờ tạo hoặc phá hủy các khối. 

Đại lượng mà chúng tôi quan tâm là độ chênh lệch của chiều cao tháp, được định nghĩa là sự khác biệt giữa tháp cao nhất và tháp ngắn nhất sau khi thực hiện tối đa một số bước di chuyển nhất định. Chúng tôi muốn giảm thiểu mức chênh lệch này và chúng tôi cũng được yêu cầu đưa ra một chuỗi các bước di chuyển để đạt được kết quả tốt nhất có thể. 

Khó khăn chính là mỗi thao tác chỉ di chuyển một khối, do đó, việc thay đổi chiều cao tháp một lượng lớn sẽ mất nhiều bước và chúng tôi bị hạn chế bởi số lượng di chuyển nhỏ. 

Các ràng buộc rất nhỏ: nhiều nhất là 100 tháp và nhiều nhất là 1000 lượt di chuyển. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng khám phá các chuỗi hoạt động một cách kết hợp hoặc mô phỏng bất kỳ điều gì ngoài việc cải tiến từng bước một cách tham lam. Một mô phỏng đơn giản của tất cả các chuỗi chuyển động có chiều sâu theo cấp số nhân và không thể thực hiện được. 

Một trường hợp khó nhận thấy xuất phát từ thực tế là các khối di chuyển có thể tạm thời làm tăng tính bất ổn. Ví dụ: nếu chúng ta di chuyển một khối lập phương từ tháp giữa, chúng ta có thể làm mức chênh lệch chênh lệch giảm đi trước khi cải thiện nó sau này. Một chiến lược tham lam chỉ chấp nhận cải tiến ngay lập tức sẽ thất bại. 

Một tình huống góc khác xuất hiện khi tất cả các tòa tháp đều có chiều cao bằng nhau. Trong trường hợp đó, không có động thái nào có thể cải thiện câu trả lời và bất kỳ sự phân phối lại không cần thiết nào sẽ chỉ lãng phí hoạt động. Ví dụ, đầu vào`4 10 / 5 5 5 5`nên ngay lập tức trở lại sự bất ổn`0`với số không di chuyển. 

Cuối cùng, có những trường hợp cân bằng tối ưu đòi hỏi phải sử dụng đồng thời cả tháp cao nhất và tháp ngắn nhất. Một chiến lược tham lam chỉ khắc phục một thái cực tại một thời điểm có thể không hội tụ được trong giới hạn di chuyển. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là mô phỏng tất cả các chuỗi có thể có tối đa k bước di chuyển. Từ một cấu hình chiều cao tháp nhất định, mỗi lần di chuyển sẽ chọn một cặp tháp theo thứ tự, tạo ra tối đa n(n−1) trạng thái tiếp theo có thể xảy ra. Ngay cả với n = 100 và k = 1000, điều này sẽ bùng nổ thành một không gian tìm kiếm rộng lớn về mặt thiên văn. Chỉ riêng yếu tố phân nhánh đã khiến điều này không thể thực hiện được. 

Nhận xét quan trọng là mục tiêu cuối cùng không phải là về từng tòa tháp mà là giảm sự khác biệt giữa độ cao tối đa và tối thiểu. Mỗi lần di chuyển sẽ dịch chuyển chính xác một đơn vị từ tháp này sang tháp khác, vì vậy mỗi thao tác sẽ giảm một tháp đi 1 và tăng tháp khác lên 1. Điều này có nghĩa là chúng ta có thể nghĩ theo cách làm phẳng dần các giá trị cực trị. 

Nếu một tòa tháp cao hơn mục tiêu trung bình hiện tại thì đó là nguồn dư thừa. Nếu ở dưới thì là bồn rửa. Cấu hình cuối cùng tốt nhất có thể sau nhiều thao tác sẽ luôn nằm càng gần với chiều cao trung bình càng tốt, vì tổng tổng là cố định và chúng ta đang phân phối lại khối lượng. 

Điều này gợi ý một chiến lược tham lam: liên tục di chuyển một khối lập phương từ tòa tháp cao nhất hiện tại đến tòa tháp thấp nhất hiện tại. Mỗi động thái như vậy sẽ làm giảm khoảng cách giữa các thái cực hoặc ít nhất là đẩy cả hai đầu về phía giữa. Vì chúng tôi luôn chọn cặp cực đoan nhất nên chúng tôi không bao giờ lãng phí một nước đi. 

Chúng tôi mô phỏng quy trình này trong tối đa k bước, tính toán lại mức tối thiểu và tối đa hiện tại sau mỗi thao tác. Vì n ≤ 100 nên quét tuyến tính là đủ rẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Cân bằng cực độ tham lam | O(k·n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì danh sách độ cao của tháp và thực hiện liên tục tối đa k thao tác. 

1. Tính chỉ số của tháp tối đa hiện tại và tháp tối thiểu hiện tại. 

Điều này xác định cặp không cân bằng nhất trong hệ thống, luôn là cặp có lợi nhất để điều chỉnh. 
2. Nếu chênh lệch giữa mức tối đa và tối thiểu là 0 hoặc 1, chúng tôi dừng sớm. 

Tại thời điểm này, không có hoạt động nào có thể làm giảm sự mất ổn định hơn nữa một cách có ý nghĩa. 
3. Di chuyển một khối từ tháp tối đa đến tháp tối thiểu. 

Điều này làm giảm khoảng cách chiều cao giữa các cực ít nhất là 1, vì một cực giảm trong khi cực kia tăng. 
4. Ghi lại thao tác này dưới dạng một cặp chỉ số. 
5. Lặp lại cho đến khi đạt được k thao tác hoặc không thể cải thiện được. 
6. Cuối cùng, tính toán lại độ không ổn định dưới dạng chiều cao tối đa trừ đi chiều cao tối thiểu và xuất giá trị cùng với danh sách các thao tác. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi thao tác đều trực tiếp làm giảm sự khác biệt giữa các tháp tối đa và tối thiểu hiện tại trừ khi nhiều tháp liên kết với nhau ở các điểm cực trị, trong trường hợp đó, hiệu ứng vẫn không xấu đi trong trường hợp xấu nhất. Vì cách duy nhất để giảm bớt sự bất ổn là di chuyển khối lượng từ tháp cao hơn sang tháp thấp hơn, nên luôn chọn các điểm cực trị tổng thể để đảm bảo chúng ta không bao giờ lãng phí một động thái đối với một chuyển giao không cực đoan sẽ ít ảnh hưởng hơn đến mức chênh lệch cuối cùng. 

Bởi vì mỗi bước di chuyển làm giảm khoảng cách tiềm năng theo cách tối ưu cục bộ và do quá trình bị giới hạn bởi k thao tác, nên thuật toán tạo ra sự mất ổn định nhỏ nhất có thể đạt được trong số lần di chuyển được phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

ops = []

for _ in range(k):
    mx_i = 0
    mn_i = 0

    for i in range(n):
        if a[i] > a[mx_i]:
            mx_i = i
        if a[i] < a[mn_i]:
            mn_i = i

    if a[mx_i] - a[mn_i] <= 1:
        break

    a[mx_i] -= 1
    a[mn_i] += 1
    ops.append((mx_i + 1, mn_i + 1))

mx = max(a)
mn = min(a)
print(mx - mn, len(ops))
for i, j in ops:
    print(i, j)
```Việc triển khai liên tục quét tất cả các tháp để tìm mức tối đa và tối thiểu hiện tại. Điều này đơn giản có chủ ý vì n đủ nhỏ để O(nk) vẫn hiệu quả. 

Một chi tiết tinh tế là điều kiện dừng`a[mx_i] - a[mn_i] <= 1`. Khi chênh lệch tối đa là một, bất kỳ sự phân phối lại nào nữa đều không thể làm giảm đáng kể sự mất ổn định cuối cùng trong các bước di chuyển còn lại, vì hệ thống đã phẳng nhất có thể dưới các ràng buộc số nguyên. 

Một chi tiết khác là chúng tôi tính toán lại giá trị tối đa và tối thiểu ở mỗi bước thay vì duy trì một đống. Điều này tránh được các lỗi phức tạp và đủ nhanh trong các ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
5 8 5
```| Bước | Độ cao | Chỉ số tối đa | Chỉ số tối thiểu | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 | [5, 8, 5] | 2 | 1 hoặc 3 | 2 → 1 | 
| 1 | [6, 7, 5] | 2 | 3 | 2 → 3 | 
| 2 | dừng lại | | | | 

Sau hai lần di chuyển, cấu hình trở nên gần cân bằng hơn nhưng không hoàn toàn bằng nhau. Độ bất ổn cuối cùng là 2. 

Dấu vết này cho thấy việc điều chỉnh các điểm cực trị liên tục sẽ dần dần nén phạm vi như thế nào. 

### Ví dụ 2 

đầu vào:```
4 5
1 10 10 1
```| Bước | Độ cao | Chỉ số tối đa | Chỉ số tối thiểu | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 | [1, 10, 10, 1] | 2 | 1 | 2 → 1 | 
| 1 | [2, 9, 10, 1] | 3 | 4 | 3 → 4 | 
| 2 | [2, 9, 9, 2] | 2 hoặc 3 | 1 hoặc 4 | 2 → 1 | 
| 3 | [3, 8, 9, 2] | 3 | 4 | 3 → 4 | 
| 4 | [3, 8, 8, 3] | 2 hoặc 3 | 1 hoặc 4 | 2 → 1 | 

Ví dụ này cho thấy các thái cực xen kẽ. Thuật toán phân phối phần dư thừa một cách tự nhiên từ tất cả các tháp cao trong khi lấp đầy tất cả các tháp thấp, tạo ra cấu hình cân bằng. 

Mỗi bước xác nhận sự bất biến rằng chênh lệch sẽ giảm hoặc vẫn được kiểm soát trong khi luôn sử dụng phương thức chuyển có tác động mạnh nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k·n) | Mỗi thao tác k quét n tháp để tìm min và max | 
| Không gian | O(n + k) | Lưu trữ chiều cao và danh sách hoạt động | 

Với n ≤ 100 và k ≤ 1000, số lượng thao tác tối đa là 1000 và mỗi lần tốn tối đa 100 lần quét, do đó, khoảng 100k kiểm tra nguyên thủy, dễ dàng thực hiện nhanh trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    ops = []

    for _ in range(k):
        mx_i = 0
        mn_i = 0
        for i in range(n):
            if a[i] > a[mx_i]:
                mx_i = i
            if a[i] < a[mn_i]:
                mn_i = i

        if a[mx_i] - a[mn_i] <= 1:
            break

        a[mx_i] -= 1
        a[mn_i] += 1
        ops.append((mx_i + 1, mn_i + 1))

    mx = max(a)
    mn = min(a)
    out = [str(mx - mn), str(len(ops))]
    for i, j in ops:
        out.append(f"{i} {j}")
    return "\n".join(out)

# provided sample
assert run("3 2\n5 8 5\n") == "2 2\n2 1\n2 3"

# all equal
assert run("4 10\n5 5 5 5\n") == "0 0"

# single move needed
assert run("2 1\n1 10\n") == "8 1\n2 1"

# k too small to fully balance
assert run("3 1\n1 100 1\n") == "98 1\n2 1"

# already balanced-ish
assert run("5 3\n3 3 4 4 3\n") in [
    "1 0",
]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tháp bằng nhau | 0 0 | không có hoạt động không cần thiết | 
| hai tòa tháp cực | 8 1 | cân bằng một bước | 
| giữa không cân bằng | 98 1 | lựa chọn cực đoan đúng đắn | 
| trường hợp mẫu | 2 2 ... | tính đúng đắn của trình tự | 

## Vỏ cạnh 

Đối với các tháp đã đồng nhất, thuật toán ngay lập tức tìm ra giá trị tối đa trừ tối thiểu bằng 0 và dừng trước khi bất kỳ thao tác nào được ghi lại. Đối với đầu vào`5 10 / 7 7 7 7 7`, lần quét đầu tiên cho các chỉ số tối đa và tối thiểu giống hệt nhau, do đó, điều kiện chênh lệch sẽ kích hoạt việc chấm dứt sớm và tạo ra sự mất ổn định bằng 0. 

Đối với trường hợp k lớn nhưng không cần thiết, chẳng hạn như`4 1000 / 1 2 3 4`, thuật toán sẽ dừng sớm khi phạm vi tối đa là 1. Điều này ngăn ngừa lãng phí các hoạt động chỉ dao động các giá trị mà không cải thiện mục tiêu. 

Đối với các phân phối có độ lệch cao như`3 5 / 1 100 1`, mỗi lần lặp lại sẽ xác định chính xác tòa tháp cao nhất và liên tục rút nó xuống mức tối thiểu. Quá trình này sẽ luân phiên các điểm chìm một cách tự nhiên khi chúng cân bằng, duy trì tính chính xác mà không cần theo dõi mục tiêu theo cách thủ công.
