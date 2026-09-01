---
title: "CF 104447G - Vấn đề nan giải của Kaito là gì?"
description: "Chúng tôi được cung cấp nhiều trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có một danh sách các số nguyên đại diện cho bạn bè của Kaito và giá trị đích x."
date: "2026-06-30T18:00:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104447
codeforces_index: "G"
codeforces_contest_name: "Al-Baath Collegiate Programming Contest 2023"
rating: 0
weight: 104447
solve_time_s: 63
verified: true
draft: false
---

[CF 104447G - Dị tật của Kaito là gì?](https://codeforces.com/problemset/problem/104447/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có một danh sách các số nguyên đại diện cho bạn bè của Kaito và giá trị đích x. Chúng tôi muốn chọn một nhóm bạn sao cho AND theo bit của tất cả các giá trị được chọn chính xác là x và trong số tất cả các nhóm hợp lệ, chúng tôi muốn có kích thước lớn nhất có thể. Nếu không thể tạo thành bất kỳ nhóm nào có AND bằng x, chúng ta sẽ xuất ra −1. 

Thao tác phím là bitwise AND trên tất cả các số đã chọn. Thao tác này chỉ giữ một bit được đặt nếu bit đó được đặt ở mọi số đã chọn. Vì vậy, kết quả cuối cùng là giao điểm của tất cả các biểu diễn nhị phân trong tập hợp con. 

Các ràng buộc rất lớn: tổng số lên tới 5 × 10^5 trong các trường hợp thử nghiệm. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các tập hợp con hoặc thậm chí thử tất cả các cặp hoặc bộ ba. Bất cứ điều gì tệ hơn tuyến tính trên mỗi trường hợp thử nghiệm sẽ thất bại. 

Một trường hợp lỗi tinh tế xuất hiện khi x chứa các bit không có trong một số được chọn. Ví dụ: nếu x = 6 (110₂), nhưng chúng tôi chọn một số như 1 (001₂), AND ngay lập tức mất các bit cần thiết và không bao giờ có thể khôi phục chúng. Vì vậy, những con số như vậy không bao giờ có thể là một phần của tập hợp con hợp lệ. Một trường hợp quan trọng khác là khi tất cả các số có thể được sử dụng vẫn chia sẻ một số bit bổ sung không thuộc x. Trong trường hợp đó, không tập hợp con nào có thể loại bỏ bit đó khỏi AND, vì việc loại bỏ các phần tử chỉ làm giảm AND và nếu mọi phần tử đều có tập hợp bit đó thì nó sẽ luôn tồn tại. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử từng tập hợp con của bạn bè, tính toán AND theo bit cho mỗi tập hợp con và theo dõi kích thước lớn nhất có kết quả bằng x. Điều này đúng vì nó khám phá mọi khả năng. Vấn đề là số lượng tập hợp con theo cấp số nhân trong n và thậm chí với n = 40, điều này đã trở nên không khả thi, trong khi ở đây n tăng lên 10^5. 

Một quan sát có cấu trúc hơn xuất phát từ việc hiểu cách hành xử của AND. Một số chỉ có thể tham gia vào một tập hợp con hợp lệ nếu nó không bỏ sót bất kỳ bit nào mà x yêu cầu. Điều đó có nghĩa là mọi giá trị được chọn phải thỏa mãn (ai & x) = x. Điều này ngay lập tức lọc đầu vào xuống nhóm ứng viên. 

Bây giờ chúng tôi chỉ làm việc với nhóm đã lọc này. Nếu chúng ta lấy tất cả chúng, AND của chúng là cố định. Nếu AND này đã bằng x thì việc lấy tất cả các ứng cử viên là tối ưu, vì việc thêm nhiều phần tử hơn chỉ làm cho AND nhỏ hơn hoặc bằng bit chứ không bao giờ lớn hơn. Vì vậy, nếu chúng ta đã ở chính xác ở x, chúng ta nên giữ lại mọi thứ. 

Nếu AND của tất cả các ứng viên hợp lệ không bằng x thì tồn tại ít nhất một bit là 1 trong tất cả các ứng viên ngoại trừ 0 trong x. Bit đó không bao giờ có thể được loại bỏ bằng cách loại bỏ một số phần tử, bởi vì việc loại bỏ không tạo ra các số 0 ở những nơi không tồn tại. Trong trường hợp đó, không có tập con hợp lệ nào tồn tại cả. 

Điều này thu gọn vấn đề thành một lần truyền qua mảng với tính năng lọc theo bit và phép tính AND cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các tập hợp con | O(2^n · n) | O(1) | Quá chậm | 
| Lọc + kiểm tra VÀ toàn cầu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và giảm vấn đề thành bước lọc và tổng hợp đơn giản. 

### Các bước 

1. Đọc n và x và mảng a. 
2. Loại bỏ mọi giá trị ai không thỏa mãn (ai & x) = x. 

Bước này đảm bảo chúng ta không bao giờ mất các bit x cần thiết trong AND cuối cùng. 
3. Nếu không còn giá trị nào sau khi lọc, xuất −1. 
4. Tính toán AND theo từng bit của tất cả các giá trị còn lại. 
5. Nếu AND cuối cùng này bằng x, xuất ra số phần tử còn lại. 
6. Ngược lại xuất ra −1. 

### Tại sao nó hoạt động 

Bất kỳ tập hợp con hợp lệ nào cũng chỉ được bao gồm các số chứa tất cả các bit của x, vì một vi phạm duy nhất sẽ phá hủy x trong AND cuối cùng. Vì vậy, sàng lọc không phải là hạn chế mà là điều kiện cần cho tính khả thi.

Khi chúng tôi giới hạn bản thân ở các số hợp lệ, AND của bất kỳ tập hợp con nào chỉ có thể lớn hơn hoặc bằng AND của tập hợp đầy đủ về mặt ngăn chặn bit, bởi vì việc loại bỏ các phần tử chỉ có thể biến 1 bit thành 0 bit. Điều này có nghĩa là bộ lọc đầy đủ sẽ mang lại AND mạnh nhất có thể. Nếu ngay cả AND mạnh nhất này không khớp với x thì không có tập hợp con nào có thể sửa được các bit bị thiếu hoặc thừa, do đó không có giải pháp nào tồn tại. Nếu nó khớp với x, việc loại bỏ các phần tử sẽ chỉ làm suy yếu AND và di chuyển ra khỏi x, vì vậy giữ tất cả là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        arr = list(map(int, input().split()))

        filtered = []
        for v in arr:
            if (v & x) == x:
                filtered.append(v)

        if not filtered:
            print(-1)
            continue

        cur = filtered[0]
        for v in filtered[1:]:
            cur &= v

        if cur == x:
            print(len(filtered))
        else:
            print(-1)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh lý do trực tiếp. Bước lọc thực thi điều kiện cần thiết là mọi số được chọn phải chứa tất cả các bit của x. Vòng lặp thứ hai tính toán AND toàn cục của tất cả các ứng cử viên còn lại, đại diện cho AND mạnh nhất có thể đạt được từ bất kỳ tập hợp con nào. 

Một cạm bẫy phổ biến là cố gắng thông minh về các tập hợp con sau khi lọc, nhưng điều đó là không cần thiết. Bản chất đơn điệu của bitwise AND đảm bảo rằng toàn bộ bộ lọc hoạt động hoặc không có gì hoạt động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 4, x = 2 

a = [2, 4, 7, 6] 

Đầu tiên chúng ta lọc theo (ai & x) = x. Vì x = 2 (010₂), chúng tôi chỉ giữ lại các số có tập bit thứ hai. Điều đó mang lại [2, 7, 6]. 

Bây giờ chúng tôi tính toán AND của họ từng bước một. 

| Bước | Hiện tại VÀ | 
| --- | --- | 
| Bắt đầu | 2 (010) | 
| VÀ với 7 | 2 (010) | 
| VÀ với 6 | 2 (010) | 

Kết quả cuối cùng là 2, khớp với x, vì vậy chúng tôi xuất ra 3. 

Điều này cho thấy rằng việc giữ lại tất cả các ứng viên hợp lệ sẽ duy trì đủ cơ cấu để đạt được mục tiêu. 

### Ví dụ 2 

đầu vào: 

n = 3, x = 3 

a = [1, 2, 3] 

Lọc với (ai & 3) = 3 chỉ giữ lại 3, vì cả 1 và 2 đều bỏ lỡ các bit bắt buộc. 

Bây giờ đã lọc = [3]. AND là 3, khớp với x nên đáp án là 1. 

Điều này chứng tỏ rằng thuật toán xử lý chính xác các trường hợp trong đó hầu hết các phần tử đều không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được kiểm tra một lần và sau đó được kết hợp thông qua một thẻ AND | 
| Không gian | O(n) trường hợp xấu nhất | Danh sách lọc cửa hàng | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính ở kích thước đầu vào, vừa vặn thoải mái trong giới hạn tổng số phần tử 5 × 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    output = []

    def input():
        return sys.stdin.readline()

    t = int(sys.stdin.readline())
    for _ in range(t):
        n, x = map(int, sys.stdin.readline().split())
        arr = list(map(int, sys.stdin.readline().split()))

        filtered = [v for v in arr if (v & x) == x]
        if not filtered:
            output.append("-1")
            continue

        cur = filtered[0]
        for v in filtered[1:]:
            cur &= v

        output.append(str(len(filtered) if cur == x else -1))

    return "\n".join(output)

assert run("""1
6 0
7 3 5 2 8 4
""") == "6"

assert run("""1
4 2
2 4 7 6
""") == "3"

assert run("""1
2 1
3 7
""") == "-1"

assert run("""1
3 3
3 3 3
""") == "3"

assert run("""1
3 3
1 2 3
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| x = 0 trường hợp | tất cả đều hợp lệ được tính | xử lý ràng buộc zero-bit | 
| giá trị hỗn hợp | tính chính xác của việc lọc một phần | bộ lọc + hành vi AND | 
| trường hợp bất khả thi | -1 | không có tập hợp con hợp lệ | 
| tất cả đều có giá trị như nhau | chấp nhận hoàn toàn | trường hợp ổn định | 
| giá trị hỗn hợp | sự thống trị lọc chính xác | logic lọc cạnh | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi x = 0. Trong tình huống này, mọi số tự động thỏa mãn (ai & x) = x vì x không có bit nào được đặt. Tập hợp được lọc sẽ trở thành toàn bộ mảng và AND của tất cả các số xác định xem có tồn tại bất kỳ ràng buộc bổ sung nào hay không. Nếu tất cả các số có chung một bit, AND sẽ không bằng 0 và câu trả lời trở thành −1; nếu không tất cả các yếu tố có thể được thực hiện. 

Một trường hợp quan trọng khác là khi quá trình lọc loại bỏ mọi thứ. Ví dụ: nếu x yêu cầu một bit mà không có số nào chứa thì không có phần tử nào có thể thỏa mãn (ai & x) = x. Thuật toán xuất ra chính xác −1 ngay lập tức mà không cần thực hiện bất kỳ tính toán nào thêm. 

Trường hợp tinh vi cuối cùng xảy ra khi tập hợp được lọc không trống nhưng tất cả các phần tử đều có chung một bit chung bổ sung không có trong x. Trong trường hợp này, AND toàn cục giữ lại bit đó và vì không có tập hợp con nào có thể loại bỏ nó nên câu trả lời cuối cùng là −1.
