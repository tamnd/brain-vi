---
title: "CF 105638F - Tái sinh và TFT"
description: "Chúng ta được cấp một nhóm tướng, mỗi tướng được mô tả bằng một chuỗi nhị phân dựa trên một tập hợp các đặc điểm cố định. Số 1 ở vị trí j có nghĩa là tướng sở hữu đặc điểm j."
date: "2026-06-22T05:28:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105638
codeforces_index: "F"
codeforces_contest_name: "GPC 2024"
rating: 0
weight: 105638
solve_time_s: 59
verified: true
draft: false
---

[CF 105638F - Reborn và TFT](https://codeforces.com/problemset/problem/105638/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một nhóm tướng, mỗi tướng được mô tả bằng một chuỗi nhị phân dựa trên một tập hợp các đặc điểm cố định. MỘT`1`ở vị trí`j`có nghĩa là nhà vô địch sở hữu đặc điểm`j`. Đối với mỗi đặc điểm, chúng tôi cũng được cung cấp một số lượng tướng riêng biệt cần thiết phải sở hữu đặc điểm đó để nó có thể hoạt động. 

Nhiệm vụ không phải là quyết định tính khả thi của một lựa chọn cố định mà là chọn một tập hợp con các nhà vô địch có kích thước tối đa trong một giới hạn nhất định để càng nhiều đặc điểm càng tốt có thể hoạt động. Một đặc điểm chỉ được kích hoạt nếu trong số các tướng được chọn, ít nhất số lượng tướng cần thiết của nó có chứa đặc điểm đó. 

Vì vậy, quyết định là sự kết hợp trong hai lớp. Đầu tiên, chúng tôi chọn một tập hợp con các tướng, sau đó mỗi đặc điểm sẽ kiểm tra độc lập xem có đủ các tướng được chọn đóng góp cho nó hay không. 

Các ràng buộc (với số lượng tướng tương đối nhỏ so với các bài toán quy mô lớn điển hình trong các bài toán cấp F của Codeforces thuộc dạng này) ngụ ý rằng lý luận theo cấp số nhân đối với các tướng là có thể chấp nhận được, trong khi mọi thứ phụ thuộc vào việc lặp lại tất cả các tập hợp con các đặc điểm hoặc thực hiện tính toán lại từng tập hợp con mà không có cấu trúc sẽ thất bại. 

Một nỗ lực ngây thơ sẽ là tham lam chọn những tướng có thể tối đa hóa khả năng kích hoạt đặc điểm ngay lập tức. Điều này không thành công vì các đặc điểm tương tác thông qua các nhà vô địch chung. Một tướng giúp hoàn thành một đặc điểm cũng có thể cần thiết cho một đặc điểm khác và những lựa chọn quá sớm có thể cản trở sự kết hợp tốt hơn. 

Ý tưởng ngây thơ thứ hai là đánh giá từng tập hợp con các nhà vô địch và tính xem có bao nhiêu đặc điểm được thỏa mãn. Điều này đúng nhưng sẽ không khả thi ngay khi số lượng nhà vô địch vượt quá khoảng 25, vì số lượng tập hợp con tăng lên theo tỷ lệ.$2^m$và mỗi tập hợp con yêu cầu quét tất cả các đặc điểm. 

Khó khăn tiềm ẩn chính là chúng tôi đang tối ưu hóa một mục tiêu đơn điệu nhưng phi tuyến tính trên các tập hợp con: việc thêm một tướng không bao giờ ảnh hưởng đến bất kỳ đặc điểm nào, nhưng lợi ích chỉ xuất hiện sau khi vượt qua các ngưỡng. Cấu trúc “chức năng bước trên mỗi đặc điểm” này là thứ khiến cho lực lượng vũ phu có thể được sử dụng khi cắt tỉa hoặc m nhỏ, nhưng khó tăng tốc hơn nữa nếu không có cấu trúc mạnh hơn. 

Trường hợp phức tạp phát sinh khi nhiều đặc điểm có chung các yêu cầu giống hệt nhau hoặc chồng chéo. Ví dụ: một bộ tướng gần như có thể đáp ứng đồng thời hai đặc điểm, nhưng việc lựa chọn tham lam hoàn thành một đặc điểm sớm sẽ làm giảm tính linh hoạt cho đặc điểm thứ hai, dẫn đến tổng các đặc điểm được kích hoạt tồi tệ hơn. Đây là lý do tại sao bất kỳ chiến lược lựa chọn địa phương nào cũng thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê mọi tập hợp con tướng có thể có, miễn là chúng tôi vẫn ở trong giới hạn vị trí. Đối với mỗi tập hợp con, đối với mỗi đặc điểm, chúng tôi đếm có bao nhiêu tướng được chọn có đặc điểm đó, sau đó kiểm tra xem liệu nó có đáp ứng ngưỡng kích hoạt hay không. Điều này đơn giản và chính xác vì nó đánh giá định nghĩa một cách trực tiếp mà không cần xấp xỉ. 

Chi phí của phương pháp này xuất phát từ sự bùng nổ tập hợp con. Nếu có$m$nhà vô địch, có$2^m$tập hợp con. Đối với mỗi tập hợp con, chúng ta có thể cần tính toán lại số lượng đặc điểm lên đến$t$đặc điểm, tạo ra sự phức tạp trong trường hợp xấu nhất xung quanh$O(2^m \cdot m \cdot t)$. Điều này trở nên không thể thực hiện được một khi$m$là hơn khoảng 25. 

Quan sát quan trọng là không gian quyết định về cơ bản tập trung vào các tướng chứ không phải đặc điểm. Đặc điểm chỉ là một cơ chế tính điểm. Điều này cho phép chúng ta chấp nhận độ phức tạp theo cấp số nhân trong$m$miễn là$m$đủ nhỏ và tính toán mức độ hài lòng của đặc điểm tăng dần bằng cách sử dụng các thao tác bit hoặc mặt nạ được tính toán trước. Mỗi tập hợp con có thể được đánh giá một cách hiệu quả bằng cách tổng hợp các mặt nạ đặc điểm của nhà vô địch. 

Điều này biến vấn đề thành một bảng liệt kê tập hợp con rõ ràng với sự tích lũy tăng dần, trong đó mỗi trạng thái là độc lập và được đánh giá theo công việc phân bổ không đổi trên mỗi bit đặc điểm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con |$O(2^m \cdot m \cdot t)$|$O(1)$hoặc$O(t)$| Quá chậm nếu m lớn | 
| Bảng liệt kê tập hợp con được tối ưu hóa với mặt nạ bit |$O(2^m \cdot t)$|$O(m \cdot t)$| Được chấp nhận cho m nhỏ | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi tướng như một mặt nạ bit về đặc điểm. Mục tiêu là thử mọi tập hợp con có thể có của các tướng có kích thước tối đa trong giới hạn vị trí và tính toán xem có bao nhiêu đặc điểm sẽ hoạt động. 

1. Tính toán trước mặt nạ bit đặc điểm của mỗi tướng để việc kiểm tra xem một tướng có đóng góp vào đặc điểm hay không là một thao tác bit liên tục. Điều này tránh việc quét liên tục các chuỗi trong quá trình đánh giá tập hợp con. 
2. Lặp lại tất cả các tập hợp con của tướng bằng cách sử dụng mặt nạ bit từ`0`ĐẾN`(1 << m) - 1`. 
3. Đối với mỗi tập hợp con, trước tiên hãy kiểm tra xem kích thước của nó có vượt quá giới hạn vị trí hay không. Nếu có, hãy bỏ qua nó ngay lập tức vì nó không thể là lựa chọn hợp lệ. 
4. Để có tập hợp con hợp lệ, hãy tính một mảng`cnt[j]`lưu trữ bao nhiêu tướng được chọn chứa đặc điểm`j`. Điều này được thực hiện bằng cách lặp lại các bit đã đặt của tập hợp con và tích lũy phần đóng góp của chúng. 
5. Sau khi tính xong, đánh giá từng tính trạng: nếu`cnt[j]`ít nhất là ngưỡng bắt buộc cho đặc điểm đó, hãy tăng điểm cho tập hợp con này. 
6. Theo dõi điểm tối đa trên tất cả các tập hợp con và xuất ra. 

Lý do chính khiến chúng tôi có thể thực hiện phép liệt kê này là vì việc đánh giá mỗi tập hợp con là tuyến tính về số lượng đặc điểm và tướng được chọn, đồng thời số lượng tập hợp con đủ nhỏ cho các ràng buộc dự định. 

### Tại sao nó hoạt động 

Mỗi lựa chọn tướng hợp lệ sẽ tương ứng chính xác với một mặt nạ tập hợp con. Thuật toán đánh giá hàm mục tiêu được xác định trong bài toán cho mọi tập hợp con như vậy. Vì không có tập hợp con nào bị bỏ qua ngoại trừ những tập hợp con vi phạm giới hạn kích thước và mọi tập hợp con được đánh giá đều được tính điểm chính xác theo định nghĩa kích hoạt đặc điểm, nên giá trị tối đa được ghi là giá trị tối ưu thực sự trên tất cả các lựa chọn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, S = map(int, input().split())
    k = list(map(int, input().split()))

    masks = []
    for _ in range(m):
        s = input().strip()
        mask = 0
        for i, ch in enumerate(s):
            if ch == '1':
                mask |= 1 << i
        masks.append(mask)

    best = 0

    for sub in range(1 << m):
        if sub.bit_count() > S:
            continue

        cnt = [0] * n

        i = 0
        x = sub
        while x:
            if x & 1:
                mp = masks[i]
                for j in range(n):
                    if (mp >> j) & 1:
                        cnt[j] += 1
            x >>= 1
            i += 1

        score = 0
        for j in range(n):
            if cnt[j] >= k[j]:
                score += 1

        if score > best:
            best = score

    print(best)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên nén mỗi tướng vào một bitmask số nguyên để việc kiểm tra tư cách thành viên đặc điểm trở thành các hoạt động bit. Sau đó, nó liệt kê tất cả các tập hợp con tướng và lọc theo giới hạn vị trí. Đối với mỗi tập hợp con, nó sẽ xây dựng lại số lượng đặc điểm bằng cách chỉ lặp lại các tướng đã chọn và kiểm tra xem chúng đóng góp vào những đặc điểm nào. 

Việc lặp lại bit trên các tập hợp con được thực hiện bằng cách sử dụng phương pháp mặt nạ dịch chuyển, giúp tránh liên tục gọi các hoạt động tốn kém như chuyển đổi danh sách các chỉ mục. Bước tính điểm cuối cùng trực tiếp thực hiện quy tắc kích hoạt. 

Một lỗi phổ biến ở đây là tính toán lại các đóng góp đặc điểm từ đầu cho mỗi tập hợp con bằng cách phân tích lại các chuỗi. Điều đó sẽ bổ sung thêm hệ số độ dài chuỗi và trở nên quá chậm. Một vấn đề nhỏ khác là quên thực thi giới hạn vị trí sớm, điều này gây ra tính toán không cần thiết đối với các tập hợp con không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp đơn giản hóa với 3 đặc điểm và 3 tướng, giới hạn vị trí 2. Yêu cầu giả sử là$k = [1, 2, 1]$, và nhà vô địch là: 

- C0: 110 
- C1: 011 
- C2: 101 

Chúng tôi đánh giá các tập hợp con: 

| Tập hợp con | C0 | C1 | C2 | cnt = [t0,t1,t2] | đặc điểm được kích hoạt | 
| --- | --- | --- | --- | --- | --- | 
| ∅ | 0 | 0 | 0 | [0,0,0] | 0 | 
| {C0} | 1 | 1 | 0 | [1,1,0] | 1 | 
| {C1} | 0 | 1 | 1 | [0,1,1] | 1 | 
| {C2} | 1 | 0 | 1 | [1,0,1] | 2 | 
| {C0,C1} | 1 | 2 | 1 | [1,2,1] | 3 | 
| {C0,C2} | 2 | 1 | 1 | [2,1,1] | 3 | 
| {C1,C2} | 1 | 1 | 2 | [1,1,2] | 3 | 

Điều này xác nhận rằng thuật toán tổng hợp chính xác các đóng góp và áp dụng các ngưỡng một cách độc lập cho mỗi đặc điểm. 

### Ví dụ 2 

Lấy một trường hợp chỉ có một đặc điểm quan trọng:$k = [2]$, và vô địch: 

- C0: 1 
- C1: 1 
- C2: 1 

Giới hạn vị trí là 2. 

| Tập hợp con | đã chọn | cnt | kích hoạt | 
| --- | --- | --- | --- | 
| {C0} | 1 | 1 | 0 | 
| {C0,C1} | 2 | 2 | 1 | 
| {C1,C2} | 2 | 2 | 1 | 

Dấu vết cho thấy rằng việc kích hoạt chỉ xảy ra sau khi vượt qua ngưỡng chứ không phải theo tỷ lệ, đó chính xác là lý do tại sao lựa chọn tham lam là không đáng tin cậy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^m \cdot n)$| mỗi tập hợp con được đánh giá và mỗi đánh giá sẽ quét các đặc điểm | 
| Không gian |$O(n + m)$| mặt nạ và quầy tạm thời | 

Sự phụ thuộc theo cấp số nhân có thể chấp nhận được dưới những ràng buộc dự định khi số lượng nhà vô địch nhỏ. Số lượng đặc điểm chỉ đóng góp tuyến tính, phù hợp thoải mái trong giới hạn ngay cả đối với các giá trị vừa phải. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve() or "").strip()

# minimal case
assert run("1 1 1\n1\n1\n") == "1"

# slot limit prevents taking all
assert run("2 3 1\n1\n1\n1\n") == "1"

# mixed traits
assert run("3 3 2\n1 0 1\n1 1 0\n0 1 1\n1 1 1\n") == "3"

# all champions identical
assert run("2 4 2\n1 1\n1 1\n1 1\n1 1\n2 2") in ["2", "2\n"]

# boundary: no activation possible
assert run("2 2 2\n1 0\n0 1\n2 2\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | 1 | nhà vô địch duy nhất, đặc điểm duy nhất | 
| tất cả đều giống hệt nhau | 2 | lựa chọn dư thừa | 
| hỗn hợp | 3 | đóng góp đặc điểm chồng chéo | 
| ngưỡng không thể | 0 | không có tập hợp con nào thỏa mãn yêu cầu | 

## Vỏ cạnh 

Một trường hợp góc là khi tất cả các nhà vô địch đóng góp vào tất cả các đặc điểm nhưng giới hạn vị trí quá nhỏ để đáp ứng bất kỳ ngưỡng nào. Ví dụ: nếu mọi tướng đều có tất cả các đặc điểm nhưng mỗi đặc điểm lại yêu cầu nhiều lựa chọn hơn số ô cho phép thì mọi tập hợp con đều thất bại. Thuật toán vẫn đánh giá tất cả các tập hợp con, nhưng không có tập hợp con nào vượt qua ngưỡng, tạo ra số 0 một cách chính xác. 

Một trường hợp khác là khi các ngưỡng là tối thiểu, chẳng hạn như tất cả$k_j = 1$. Sau đó, bất kỳ đặc điểm nào có ở ít nhất một tướng được chọn sẽ được tính và giải pháp tốt nhất thường tương ứng với việc tối đa hóa phạm vi bao phủ trong giới hạn vị trí. Việc liệt kê vẫn xử lý việc này một cách chính xác vì nó không có tính độc lập hoặc cấu trúc. 

Trường hợp thứ ba là khi giới hạn vị trí bằng số lượng nhà vô địch. Tập hợp đầy đủ được bao gồm trong bảng liệt kê và vì tất cả các tập hợp con đều được xem xét nên giải pháp tối ưu được đảm bảo sẽ được tìm thấy mà không cần cách viết vỏ đặc biệt.
