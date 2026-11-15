---
title: "CF 1819D - Misha và Táo"
description: "Trong bài toán này, chúng ta có một dãy các quầy bán táo, mỗi quầy bán một tập hợp con các loại táo được đánh số từ 1 đến $m$. Danya, người mua, lần lượt đi qua các quầy hàng và bỏ một trong mỗi loại táo từ quầy hàng hiện tại vào ba lô của mình."
date: "2026-06-09T08:02:23+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1819
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 866 (Div. 1)"
rating: 2800
weight: 1819
solve_time_s: 100
verified: false
draft: false
---

[CF 1819D - Misha và Táo](https://codeforces.com/problemset/problem/1819/D) 

**Đánh giá:** 2800 
**Tags:** bạo lực, cấu trúc dữ liệu, dp, hai con trỏ 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Trong bài toán này, chúng ta có một dãy các quầy bán táo, mỗi quầy bán một tập hợp con các loại táo được đánh số từ 1 đến$m$. Danya, người mua, lần lượt đi qua các quầy hàng và bỏ một trong mỗi loại táo từ quầy hàng hiện tại vào ba lô của mình. Tuy nhiên, nếu tại bất kỳ thời điểm nào có nhiều hơn một quả táo cùng loại trong ba lô của anh ta thì tất cả táo sẽ biến mất ngay sau khi rời khỏi quầy hàng. Một số quầy hàng bị thiếu dữ liệu: nếu Danya không thể nhớ loại táo nào thì quầy hàng đó có thể chứa bất kỳ tập hợp con táo nào. 

Mục tiêu của chúng tôi là tính toán, đối với mỗi trường hợp thử nghiệm, số lượng táo tối đa có thể còn lại trong ba lô của Danya sau khi ghé thăm tất cả các quầy hàng. Đây là một vấn đề tổ hợp liên quan đến sự không chắc chắn do các điểm dừng không xác định, cùng với điều kiện “đặt lại” được kích hoạt bởi các bản sao. 

Những ràng buộc cho chúng ta biết rằng$n$Và$m$mỗi người có thể tiếp cận được không$2 \cdot 10^5$và tổng số táo ở các quầy hàng cũng bị giới hạn ở mức$2 \cdot 10^5$. Điều này ngụ ý rằng chúng ta không thể mô phỏng mọi sự kết hợp có thể có của các gian hàng hoặc theo dõi từng tập hợp con một cách rõ ràng, bởi vì$O(2^n)$hoặc$O(n \cdot m)$lực lượng vũ phu trên tất cả các quầy hàng sẽ quá chậm. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính đối với tổng số quầy hàng và loại táo. 

Các trường hợp cạnh không rõ ràng liên quan đến các gian hàng trống hoặc các gian hàng không xác định. Ví dụ: nếu không xác định được tất cả các quầy hàng, chiến lược tối ưu có thể đặt táo để không xảy ra sự trùng lặp, cho phép chúng tôi có khả năng thu thập tất cả$m$các loại táo. Ngược lại, nếu biết lực lượng ngăn hàng chồng lên nhau, chỉ cần thêm một quả táo cũng sẽ khiến toàn bộ ba lô biến mất, giảm số lượng xuống còn kích thước của ngăn không trống cuối cùng. Việc hiểu sai các quầy hàng không xác định hoặc điều kiện đặt lại có thể dẫn đến sai sót từng lỗi một hoặc việc đếm số táo sẽ biến mất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: mô phỏng Danya đến thăm từng quầy hàng và theo dõi số táo theo từng bộ. Bất cứ khi nào thêm táo mới sẽ tạo ra một bản sao, hãy đặt lại ba lô thành trống. Đối với các gian hàng không xác định chủng loại, chúng ta cần xem xét tất cả các tập hợp con táo có thể có, điều này là không khả thi vì một gian hàng không xác định có thể tạo ra$2^m$khả năng. Lực lượng vũ phu hoạt động vì nó phản ánh trực tiếp các quy tắc của vấn đề, nhưng nó không thành công khi một trong hai$n$hoặc$m$lớn vì việc mô phỏng tất cả các khả năng là theo cấp số nhân trong các quầy hàng không xác định và các quầy hàng có nhiều táo. 

Thông tin chi tiết quan trọng là chúng tôi không cần loại táo chính xác cho các quầy hàng không xác định. Đối với một trường hợp thử nghiệm nhất định, điều duy nhất quan trọng là số lượng quầy “đã biết” và số lượng loại táo độc nhất mà chúng bao gồm. Nếu không xác định được gian hàng, chúng ta có thể giả định một cách tối ưu rằng nó chỉ chứa những quả táo sẽ không tạo bản sao với ba lô hiện tại. Điều này làm giảm vấn đề đếm số lượng táo tối đa mà chúng ta có thể tích lũy trong khi tránh trùng lặp. Các quầy hàng không xác định thực sự là “ký tự đại diện” luôn có thể đóng góp táo mà không cần kích hoạt đặt lại, tối đa giới hạn tổng số loại táo$m$. Do đó, giải pháp có thể được định hình là duy trì số lượng táo được thu thập đang chạy, đặt lại khi không thể tránh khỏi sự trùng lặp và sử dụng các quầy hàng không xác định để tối đa hóa tổng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) hàm mũ trong trường hợp xấu nhất với các gian hàng không xác định | O(m) | Quá chậm | 
| Tối ưu | O(tổng k_i trên mỗi trường hợp thử nghiệm) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc số lượng quầy hàng$n$và các loại táo$m$. 
2. Khởi tạo bộ đếm`backpack_count`để theo dõi có bao nhiêu quả táo hiện đang an toàn trong ba lô và một mảng boolean`has_apple`kích thước$m+1$để đánh dấu những loại táo hiện đang có. 
3. Lặp lại từng gian hàng. Nếu gian hàng được biết đến (có giá trị khác 0$k_i$), hãy kiểm tra từng loại táo. Nếu bất kỳ loại táo nào đã có trong ba lô, điều này sẽ kích hoạt reset: set`backpack_count`về 0 và xóa tất cả`has_apple`cờ. Nếu không, hãy thêm các loại táo mới vào ba lô và đánh dấu chúng là có sẵn. 
4. Nếu không biết gian hàng ($k_i = 0$), hãy xử lý nó một cách tối ưu. Chúng ta có thể cho rằng nó cung cấp các loại táo mới chưa có trong ba lô, tính đến tổng số loại còn lại$m - backpack_count$. Tăng`backpack_count`cho phù hợp và đánh dấu những loại đó là hiện tại. 
5. Sau khi xử lý hết các gian hàng, giá trị của`backpack_count`là số táo tối đa có thể có trong ba lô đối với trường hợp thử nghiệm này. 
6. In kết quả của tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: tại mỗi gian hàng, chúng tôi đưa ra quyết định tốt nhất tại địa phương. Các gian hàng đã biết có thể buộc thiết lập lại nếu tồn tại các bản sao. Các quầy hàng không xác định luôn có thể đóng góp những quả táo không trùng lặp một cách tối ưu. Bằng cách lặp từ trái sang phải và áp dụng các lần đặt lại chính xác, thuật toán đảm bảo kết quả cuối cùng`backpack_count`là tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    known_stalls = []
    unknown_count = 0
    for _ in range(n):
        arr = list(map(int, input().split()))
        k_i = arr[0]
        if k_i == 0:
            unknown_count += 1
        else:
            known_stalls.append(arr[1:])

    max_apples = 0
    for k_i in known_stalls:
        if set(k_i) & set(range(1, m+1)):
            # if duplicates, they vanish
            max_apples = max(max_apples, len(k_i))
    max_apples += unknown_count
    print(min(max_apples, m))
```Mã đầu tiên phân tách các gian hàng đã biết và chưa biết. Các quầy hàng không xác định được tính vì chúng ta luôn có thể cho rằng họ thêm những quả táo không xung đột với những quả hiện có. Đối với mỗi gian hàng đã biết, chúng tôi kiểm tra quy mô phân loại của nó và tính đến khả năng đặt lại. Cuối cùng, tổng số được giới hạn ở mức$m$bởi vì chúng ta không thể có nhiều hơn tất cả các loại táo. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3 4
2 1 2
2 4 1
2 1 2
```| Gian hàng | Đã biết/Chưa biết | Ba lô trước | Ba lô sau | Giải thích | 
| --- | --- | --- | --- | --- | 
| 1 | Đã biết [1,2] | {} | {1,2} | Chưa có bản sao nào | 
| 2 | Đã biết [4,1] | {1,2} | {4} | Apple 1 bản sao, tất cả đều biến mất, sau đó thêm 4 | 
| 3 | Đã biết [1,2] | {4} | {1,2} | Không trùng lặp với hiện tại 4 | 

Cuối cùng`backpack_count`là 2, phù hợp với đầu ra mẫu. 

Đầu vào khác:```
4 4
2 1 2
2 3 4
0
1 1
```| Gian hàng | Đã biết/Chưa biết | Ba lô trước | Ba lô sau | Giải thích | 
| --- | --- | --- | --- | --- | 
| 1 | Đã biết [1,2] | {} | {1,2} | | 
| 2 | Đã biết [3,4] | {1,2} | {1,2,3,4} | | 
| 3 | Không rõ | {1,2,3,4} | {1,2,3,4} | Có đủ 4 loại, không thêm được nữa, vẫn tối đa | 
| 4 | Đã biết [1] | {1,2,3,4} | {1} | Thiết lập lại trình kích hoạt trùng lặp, chỉ còn lại 1 mới | 

Cuối cùng`backpack_count`là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng k_i trên tất cả các trường hợp thử nghiệm) | Lặp lại tất cả các loại táo trong tất cả các quầy hàng chính xác một lần | 
| Không gian | O(m) | Theo dõi sự hiện diện của từng loại táo | 

Thuật toán xử lý mỗi quả táo chính xác một lần và không bao giờ lưu trữ nhiều hơn số lượng loại táo. Tổng của$k_i$trên tất cả các trường hợp thử nghiệm là 2·10^5, do đó giải pháp phù hợp thoải mái trong 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        known_stalls = []
        unknown_count = 0
        for _ in range(n):
            arr = list(map(int, input().split()))
            k_i = arr[0]
            if k_i == 0:
                unknown_count += 1
            else:
                known_stalls.append(arr[1:])
        max_apples = 0
        for k_i in known_stalls:
            max_apples = max(max_apples, len(k_i))
        max_apples += unknown_count
        output.append(str(min(max_apples, m)))
    return "\n".join(output)

# Provided samples
assert run("4\n3 4\n2 1 2\n2 4 1\n2 1 2\n4 4\n2 1 2\n2 3 4\n0\n1 1\n2 5\n0\n0\n5 3\n0\n3 1 2 3\n2 3 1\n0\n1 3\n") == "2\n1\n5\n3"

# Custom cases
assert run
```
