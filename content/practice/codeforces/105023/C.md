---
title: "CF 105023C - Xe chở cây"
description: "Chúng tôi được cấp nhiều chiều dài xe tải và chúng tôi muốn lắp ráp một “cấu trúc hình cây” đặc biệt bằng cách sử dụng một số chiều dài đó. Cấu trúc có một cột sống thẳng đứng được làm từ các xe tải xếp chồng lên nhau."
date: "2026-06-28T01:43:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105023
codeforces_index: "C"
codeforces_contest_name: "HPI 2024 Novice"
rating: 0
weight: 105023
solve_time_s: 92
verified: false
draft: false
---

[CF 105023C - Xe chở cây](https://codeforces.com/problemset/problem/105023/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp nhiều chiều dài xe tải và chúng tôi muốn lắp ráp một “cấu trúc hình cây” đặc biệt bằng cách sử dụng một số chiều dài đó. Cấu trúc có một cột sống thẳng đứng được làm từ các xe tải xếp chồng lên nhau. Giữa hai xe tải liên tiếp trong sống lưng này có một cấp độ phân nhánh: ở cấp độ đó, chúng ta phải gắn thêm hai xe tải có chiều dài bằng nhau, một xe bên trái và một xe bên phải. 

Thứ tự dọc của cột sống rất linh hoạt theo nghĩa là chúng ta có thể tự do lựa chọn độ dài đi đâu, nhưng các cặp nhánh đưa ra một hạn chế: chiều dài nhánh phải tuân thủ nghiêm ngặt quy tắc đơn điệu dọc theo cột sống, với các nhánh dài hơn được đặt thấp hơn và các nhánh ngắn hơn đặt cao hơn. Điều này loại bỏ bất kỳ lợi thế nào từ việc trộn các kích thước nhánh một cách tùy tiện; khi độ dài nhánh được chọn, chúng luôn có thể được sắp xếp một cách nhất quán bằng cách sắp xếp. 

Chúng tôi được phép sử dụng bất kỳ tập hợp con nào của xe tải nhất định. Mục tiêu là tối đa hóa số lượng xe tải ở cột dọc, vì điều đó quyết định chiều cao của toàn bộ kết cấu. 

Các hạn chế rất lớn, lên tới 200.000 xe tải và chiều dài lên tới 10^9. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các tập hợp con hoặc mô phỏng việc xây dựng cấu trúc một cách rõ ràng. Bất kỳ giải pháp nào về cơ bản phải là tuyến tính hoặc tuyến tính theo số lượng các giá trị riêng biệt, vì việc đếm tần số là không thể tránh khỏi. 

Một số tình huống khó khăn đáng lưu ý. Nếu tất cả các xe tải đều khác biệt, không tồn tại cặp nhánh nào, thì cấu trúc sẽ thoái hóa thành một cột có kích thước 1. Nếu tất cả các xe tải giống hệt nhau, chúng ta có thể tạo thành nhiều cặp nhánh, nhưng hệ số giới hạn là chúng ta có thể hỗ trợ bao nhiêu “cấp” đầy đủ với tổng số mặt hàng. Nếu có nhiều cặp nhưng không đủ số đơn còn sót lại, cấu trúc vẫn thất bại vì mỗi nút cột sống tiêu thụ một xe tải độc lập với việc sử dụng nhánh. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ cố gắng quyết định tập hợp con nào của xe tải tạo thành cột sống và tập hợp con nào tạo thành các cặp nhánh, sau đó kiểm tra xem liệu chúng ta có thể chỉ định các cặp có độ dài bằng nhau cho mỗi cấp theo thứ tự hợp lệ hay không. Điều này nhanh chóng trở thành tổ hợp: việc chọn một cột có kích thước k đã có nhiều khả năng và với mỗi lựa chọn như vậy, chúng ta cần xác minh xem liệu chúng ta có thể tạo thành các cặp có độ dài bằng nhau hợp lệ k−1 từ các phần tử còn lại trong khi tôn trọng các ràng buộc về thứ tự hay không. Ngay cả khi chúng ta tránh hoán vị và chỉ nghĩ đến việc ấn định tần số, số cách để phân chia giá trị thành các phần tử cột sống và các phần tử ghép đôi vẫn tăng theo cấp số nhân với N. 

Quan sát quan trọng là cấu trúc không thực sự nhạy cảm với _giá trị chính xác nào_ đi vào cột sống so với các cặp nằm ngoài các ràng buộc đếm đơn giản. Điều kiện thứ tự trên độ dài nhánh luôn có thể được thỏa mãn bằng cách sắp xếp độ dài cặp đã chọn, do đó, nó không hạn chế tính khả thi khi chúng ta biết chúng ta có bao nhiêu cặp cho mỗi kích thước. 

Điều này làm giảm vấn đề thành nhiệm vụ phân bổ đếm. Mọi cấu trúc hợp lệ với kích thước cột sống k yêu cầu chính xác k xe tải cột sống và k−1 cặp nhánh. Mỗi cặp nhánh sử dụng hai xe tải có chiều dài giống hệt nhau, vì vậy tất cả vấn đề là chúng ta có thể hình thành bao nhiêu cặp rời rạc từ nhiều tập hợp. Sau khi biết tổng số cặp có sẵn, chúng tôi chỉ cần đảm bảo có đủ vật phẩm để hỗ trợ cột sống. 

Do đó, bài toán chia thành hai ràng buộc: số cặp có thể sử dụng ít nhất phải là k−1 và tổng số xe tải còn lại phải ít nhất là k sau khi sử dụng các cặp đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng tập hợp con Brute Force | Hàm mũ | O(N) | Quá chậm | 
| Đếm tần số + ràng buộc | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đếm số lần mỗi độ dài xuất hiện trong đầu vào. Điều này là cần thiết vì việc ghép nối chỉ phụ thuộc vào tần số chứ không phụ thuộc vào danh tính. 
2. Đối với mỗi độ dài riêng biệt, hãy tính xem nó có thể đóng góp bao nhiêu cặp rời nhau, đó là sàn (đếm / 2). Tổng hợp các giá trị này để có được tổng số cặp P có sẵn. 
3. Quan sát thấy rằng một cột sống có chiều cao k yêu cầu chính xác k−1 mức nhánh, vì vậy chúng ta phải có P ≥ k−1. Điều này đưa ra ràng buộc k ≤ P + 1. 
4. Cũng quan sát rằng mỗi nút cột sống sử dụng một xe tải và mỗi cặp nhánh sử dụng hai xe tải. Một kết cấu có kích thước cột sống k tiêu thụ chính xác 2(k−1) + k = 3k−2 xe tải. Do đó chúng ta phải có 3k−2 ≤ N, suy ra k ≤ (N+2) // 3. 
5. Kết hợp cả hai ràng buộc và lấy k khả thi tối đa, là min(P + 1, (N + 2) // 3). 

### Tại sao nó hoạt động 

Khi chúng tôi sửa k, chúng tôi không còn quyết định bất kỳ điều gì mang tính cấu trúc nữa. Chúng ta chỉ cần biết liệu multiset có thể cung cấp đủ “tài nguyên” độc lập hay không: k mục đơn lẻ cho cột sống và k−1 cặp mục bằng nhau cho các nhánh. Yêu cầu thứ tự đơn điệu trên độ dài nhánh không bao giờ hạn chế tính khả thi bởi vì bất kỳ tập hợp k-1 giá trị cặp được chọn nào cũng có thể được sắp xếp để thỏa mãn điều kiện. Điều này có nghĩa là tính khả thi phụ thuộc hoàn toàn vào số lượng chứ không phải sự sắp xếp và hai bất đẳng thức bao hàm tất cả các giới hạn cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    freq = {}
    for x in arr:
        freq[x] = freq.get(x, 0) + 1

    pairs = 0
    for c in freq.values():
        pairs += c // 2

    ans = min(pairs + 1, (n + 2) // 3)
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách xây dựng bản đồ tần số trên tất cả các chiều dài xe tải, vì việc ghép nối chỉ phụ thuộc vào các giá trị giống hệt nhau. Sau đó, nó tính toán xem có thể hình thành bao nhiêu cặp rời rạc bằng cách tính tổng số tầng chia cho hai cho mỗi nhóm tần số. Cuối cùng, nó áp dụng trực tiếp các ràng buộc dẫn xuất. 

Một điểm tinh tế là chúng tôi không bao giờ xây dựng rõ ràng xe tải nào đi vào cột sống hoặc các cặp. Điều đó là không cần thiết vì bằng chứng chỉ cho thấy số lượng quan trọng và mọi phép gán số lượng hợp lệ đều có thể được thực hiện bằng cách chọn các đại diện tùy ý từ mỗi nhóm tần số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
1 1 2 2 2 3 4 5
```Chúng tôi tính toán tần số: 1→2, 2→3, 3→1, 4→1, 5→1. Điều này mang lại cặp = 1 (từ giá trị 1) + 1 (từ giá trị 2) = 2. 

Bây giờ chúng ta đánh giá các ràng buộc cho k: 

| k | P ≥ k−1 | 3k−2 8 | hợp lệ | 
| --- | --- | --- | --- | 
| 1 | vâng | vâng | vâng | 
| 2 | vâng | vâng | vâng | 
| 3 | vâng | vâng | vâng | 
| 4 | không | vâng | không | 

Vậy đáp án là 3. 

Dấu vết này cho thấy rằng mặc dù tồn tại nhiều giá trị nhưng chỉ số lượng cặp có sẵn mới giới hạn số lượng cấp độ nhánh có thể được hỗ trợ. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
```Mọi tần số đều bằng 1 nên P = 0. 

| k | P ≥ k−1 | 3k−2 5 | hợp lệ | 
| --- | --- | --- | --- | 
| 1 | vâng | vâng | vâng | 
| 2 | không | vâng | không | 

Câu trả lời là 1. 

Điều này chứng tỏ trường hợp không thể ghép cặp, buộc cấu trúc thoái hóa thành một cột sống đơn nút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Một lượt để đếm tần số và một lượt vượt qua các giá trị riêng biệt | 
| Không gian | O(N) | Bản đồ tần số trong trường hợp xấu nhất khi tất cả các giá trị đều khác nhau | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì cả bộ nhớ và thời gian đều có quy mô tuyến tính với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    from collections import Counter

    n = int(sys.stdin.readline())
    arr = list(map(int, sys.stdin.readline().split()))
    freq = Counter(arr)

    pairs = sum(v // 2 for v in freq.values())
    ans = min(pairs + 1, (n + 2) // 3)

    return str(ans)

# provided sample
assert run("8\n1 1 2 2 2 3 4 5\n") == "3"

# all distinct
assert run("5\n1 2 3 4 5\n") == "1"

# all equal
assert run("6\n7 7 7 7 7 7\n") == "2"

# minimal
assert run("1\n10\n") == "1"

# tight pairing
assert run("4\n1 1 1 1\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều khác biệt | 1 | không có cặp nào có thể | 
| tất cả đều bình đẳng | 2 | hiệu quả ghép nối tối đa | 
| kích thước tối thiểu | 1 | điều kiện biên | 
| ghép đôi chặt chẽ | 2 | tương tác của cột sống và các cặp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi đầu vào có nhiều bản sao tập trung ở một giá trị duy nhất. Ví dụ,`1 1 1 1 1 1`. Ở đây chúng tôi có sẵn 3 cặp, vì vậy P = 3. Công thức cho k = min(4, (6+2)//3 = 2), vì vậy k = 2. Mặc dù có nhiều cặp, tổng số mục hạn chế kích thước cột sống chặt chẽ hơn so với tính sẵn có của cặp. 

Một trường hợp khác là khi không có bản sao nào cả. Vì`1 2 3 4`, P = 0, vì vậy k = 1 bất kể N. Thuật toán thu gọn chính xác cấu trúc thành một phần tử cột sống duy nhất vì không thể hình thành cấp độ nhánh. 

Những trường hợp này xác nhận rằng cả hai ràng buộc, tính khả dụng của cặp và tổng tài nguyên bị ràng buộc, đều cần thiết và đủ cùng nhau một cách độc lập.
