---
title: "CF 103462I - Iaom và chân gà"
description: "Chúng ta được cho một cây, nghĩa là một đồ thị không theo chu kỳ được kết nối trên các nút $n$ có các cạnh $n-1$. Nhiệm vụ là đếm xem có bao nhiêu đồ thị con riêng biệt có hình dạng cụ thể xuất hiện bên trong cây này."
date: "2026-07-03T07:02:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103462
codeforces_index: "I"
codeforces_contest_name: "The Hangzhou Normal U Qualification Trials for ZJPSC 2021"
rating: 0
weight: 103462
solve_time_s: 45
verified: true
draft: false
---

[CF 103462I - Iaom và chân gà](https://codeforces.com/problemset/problem/103462/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cái cây, nghĩa là một đồ thị không tuần hoàn được kết nối trên$n$nút với$n-1$các cạnh. Nhiệm vụ là đếm xem có bao nhiêu đồ thị con riêng biệt có hình dạng cụ thể xuất hiện bên trong cây này. Hai đồ thị con được coi là giống hệt nhau nếu chúng bao gồm chính xác cùng một tập hợp các cạnh, vì vậy chúng ta đang đếm một cách hiệu quả các tập hợp cạnh riêng biệt tạo thành mẫu được yêu cầu. 

Mẫu này, được mô tả một cách không chính thức là “chân gà”, tương ứng với một cấu trúc nhỏ được kết nối tập trung tại một nút và phân nhánh thành ba cạnh riêng biệt, giống như một ngôi sao hình chữ Y hoặc ba nhánh. Mỗi trường hợp hợp lệ được xác định hoàn toàn bằng cách chọn một nút trung tâm và chọn ba cạnh liên quan của nó, bởi vì trong cây, bất kỳ lựa chọn nào như vậy sẽ xác định duy nhất ba nút lân cận và các cạnh nối chúng. 

Ràng buộc$n \le 5 \cdot 10^5$ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê các đồ thị con hoặc thử kết hợp các nút hoặc cạnh một cách rõ ràng. Ngay cả việc lặp lại tất cả các bộ ba cạnh cũng sẽ là quá chậm, vì số cạnh là tuyến tính theo$n$và sự kết hợp sẽ bùng nổ thành hành vi hình khối trong trường hợp xấu nhất. Do đó, giải pháp phải giảm vấn đề xuống mức có thể tính toán theo thời gian tuyến tính hoặc gần tuyến tính, lý tưởng nhất là bằng cách tổng hợp thông tin cấu trúc cục bộ tại mỗi nút. 

Trường hợp cạnh tinh tế xuất hiện khi cây nhỏ hoặc có các nút cấp độ thấp. Nếu như$n < 4$, không nút nào có thể có ba cạnh liên quan, vì vậy câu trả lời phải bằng 0. Một cạm bẫy phổ biến khác là hiểu sai những gì tạo nên một sơ đồ con riêng biệt: ngay cả khi hai ngôi sao có tâm ở các nút khác nhau nhưng trông có vẻ giống nhau về mặt cấu trúc, thì chúng vẫn khác nhau nếu tập hợp các cạnh của chúng khác nhau. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề này là cố gắng liệt kê mọi đồ thị con liên thông có thể có và kiểm tra xem nó có tạo thành cấu trúc ba hướng cần thiết hay không. Trong một cái cây có$n$các nút, số lượng đồ thị con được kết nối đã theo cấp số nhân trong trường hợp xấu nhất, vì mỗi tập hợp con của các cạnh giữ kết nối là một ứng cử viên. Ngay cả khi chúng tôi chỉ giới hạn bản thân ở các đồ thị con nhỏ có kích thước 4 nút, chúng tôi vẫn cần khám phá sự kết hợp của các cạnh và xác minh cấu trúc, điều này dẫn đến ít nhất$O(n^2)$hoặc hành vi tồi tệ hơn ở các khu vực có mật độ dày đặc. Điều này nhanh chóng trở nên không khả thi đối với$5 \cdot 10^5$nút. 

Quan sát quan trọng là cấu trúc “chân gà” hoàn toàn được xác định bởi một nút trung tâm duy nhất. Khi trung tâm đã được cố định, quyền tự do duy nhất là lựa chọn ba người hàng xóm tham gia. Trong một cây, bất kỳ ba cạnh riêng biệt nào liên quan đến một nút sẽ tự động tạo thành một sơ đồ con hợp lệ có hình dạng được yêu cầu, vì không có chu trình và mỗi cạnh dẫn đến một cây con duy nhất. Điều này thu gọn bài toán đếm toàn cục thành một bài toán tổ hợp thuần túy cục bộ. 

Đối với mỗi nút$v$có bằng cấp$\deg(v)$, số cách chọn ba cạnh liên quan là$\binom{\deg(v)}{3}$. Tính tổng số này trên tất cả các nút sẽ cho ra tổng số đồ thị con “chân gà” hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force của các sơ đồ con |$O(n^2)$ĐẾN$O(2^n)$|$O(n)$| Quá chậm | 
| Đếm kết hợp độ |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Việc tính toán trở thành một lần chuyển qua cấu trúc cây. 

1. Đọc cây và tính bậc của mỗi nút bằng cách đếm xem có bao nhiêu cạnh liên quan đến nó. Điều này là đủ vì cấu trúc mà chúng ta đang tính chỉ phụ thuộc vào sự kề cận cục bộ chứ không phụ thuộc vào cấu trúc liên kết toàn cục. 
2. Khởi tạo bộ tích lũy cho câu trả lời bằng 0. Biến này sẽ lưu trữ tổng đóng góp từ mỗi nút. 
3. Đối với mỗi nút$v$, kiểm tra mức độ của nó$d$. Nếu như$d \ge 3$, tính số cách chọn ba lân cận từ danh sách kề của nó, đó là$d(d-1)(d-2)/6$và thêm giá trị này vào câu trả lời. Các nút có bậc nhỏ hơn 3 không đóng góp gì vì chúng không thể tạo thành cấu trúc phân nhánh cần thiết. 
4. Xuất ra modulo tổng tích lũy$998244353$. 

Lý do điều này hoạt động rõ ràng là vì mọi cấu hình hợp lệ đều có chính xác một trung tâm. Một đồ thị con hình ngôi sao không thể có tâm ở nhiều hơn một nút trong cây, vì tâm là đỉnh duy nhất có bậc 3 bên trong đồ thị con đó, trong khi các đỉnh khác có bậc một. Tính duy nhất này đảm bảo rằng mọi cấu trúc hợp lệ được tính chính xác một lần khi xử lý nút trung tâm của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def nC3(x):
    if x < 3:
        return 0
    return x * (x - 1) * (x - 2) // 6

def main():
    n = int(input())
    deg = [0] * (n + 1)

    for _ in range(n - 1):
        u, v = map(int, input().split())
        deg[u] += 1
        deg[v] += 1

    ans = 0
    for i in range(1, n + 1):
        ans += nC3(deg[i])

    print(ans % MOD)

if __name__ == "__main__":
    main()
```Cốt lõi của việc triển khai là mảng độ, theo dõi cấu trúc cục bộ trong khi đọc các cạnh. Mỗi cạnh tăng hai độ, một độ cho mỗi điểm cuối, đảm bảo mảng cuối cùng nắm bắt đầy đủ thông tin lân cận. 

Hàm kết hợp sử dụng số học số nguyên trực tiếp. Vì Python xử lý các số nguyên lớn một cách an toàn nên không có vấn đề tràn, nhưng kết quả cuối cùng là giảm modulo$998244353$theo yêu cầu. 

Một chi tiết tinh tế là việc chia cho 6 được thực hiện sau khi nhân trong không gian số nguyên. Điều này an toàn vì tích của ba số nguyên liên tiếp luôn chia hết cho 6 nên không phát sinh vấn đề về độ chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một cái cây nhỏ có hình dạng giống như một nút trung tâm được nối với bốn lá. Hãy để các cạnh được$(1-2), (1-3), (1-4), (1-5)$. 

| Nút | Bằng cấp | Sự đóng góp$\binom{d}{3}$| 
| --- | --- | --- | 
| 1 | 4 | 4 | 
| 2 | 1 | 0 | 
| 3 | 1 | 0 | 
| 4 | 1 | 0 | 
| 5 | 1 | 0 | 

Câu trả lời là 4, tương ứng với việc chọn 3 trong 4 cạnh bất kỳ liên quan đến nút 1. Điều này xác nhận rằng mỗi lần chọn ba lá sẽ tạo ra một sơ đồ con riêng biệt. 

Bây giờ hãy xem xét một đường dẫn gồm năm nút:$1-2-3-4-5$. 

| Nút | Bằng cấp | Đóng góp | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 2 | 0 | 
| 3 | 2 | 0 | 
| 4 | 2 | 0 | 
| 5 | 1 | 0 | 

Mọi nút đều có bậc nhỏ hơn 3, do đó không tồn tại “chân gà” hợp lệ. Câu trả lời là 0, phù hợp với trực giác rằng một đường đi không có điểm phân nhánh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi cạnh được xử lý một lần để tính toán độ và mỗi nút được truy cập một lần để tính toán các kết hợp | 
| Không gian |$O(n)$| Mảng độ lưu trữ một số nguyên trên mỗi nút | 

Cấu trúc tuyến tính của giải pháp phù hợp thoải mái trong các ràng buộc của$5 \cdot 10^5$nút. Cả bộ nhớ và thời gian sử dụng đều tỷ lệ thuận với kích thước của biểu đồ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    MOD = 998244353

    n = int(input())
    deg = [0] * (n + 1)

    for _ in range(n - 1):
        u, v = map(int, input().split())
        deg[u] += 1
        deg[v] += 1

    ans = 0
    for i in range(1, n + 1):
        d = deg[i]
        if d >= 3:
            ans += d * (d - 1) * (d - 2) // 6

    return str(ans % MOD)

# sample-like case: star
assert run("""5
1 2
1 3
1 4
1 5
""") == "4"

# path case
assert run("""5
1 2
2 3
3 4
4 5
""") == "0"

# minimum n
assert run("""3
1 2
2 3
""") == "0"

# two centers
assert run("""7
1 2
1 3
1 4
2 5
2 6
2 7
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị sao | 4 | nhiều kết hợp tại một nút cấp cao duy nhất | 
| Biểu đồ đường dẫn | 0 | không có nút phân nhánh hợp lệ | 
| n = 3 chuỗi | 0 | trường hợp cạnh tối thiểu | 
| Hai trung tâm phân nhánh | 5 | đóng góp từ nhiều nút | 

## Vỏ cạnh 

Trong đầu vào hình ngôi sao, thuật toán gán tất cả cấu trúc cho nút trung tâm. Ví dụ: với nút trung tâm 1 được kết nối với bốn nút khác, cấp của nút 1 trở thành 4 và tất cả các nút khác vẫn là 1. Việc tính toán$\binom{4}{3} = 4$đếm chính xác tất cả các lựa chọn riêng biệt của ba cạnh và mỗi lựa chọn tương ứng với một tập cạnh duy nhất, do đó không xảy ra việc đếm quá mức. 

Trong một đường dẫn đơn giản, mọi nút đều có bậc nhiều nhất là 2, do đó công thức tổ hợp không bao giờ được kích hoạt. Thuật toán tạo ra số 0 một cách tự nhiên mà không có bất kỳ cách viết hoa đặc biệt nào, phù hợp với thực tế là không có nút nào có thể đóng vai trò là điểm phân nhánh. 

Trong một cây có nhiều nút phân nhánh, chẳng hạn như hai nút liền kề đều có mức độ cao trong các cây con khác nhau, mỗi nút đóng góp độc lập. Vì một “chân gà” hợp lệ phải có một tâm duy nhất nên không có sự trùng lặp giữa các khoản đóng góp và việc tính tổng cục bộ vẫn chính xác.
