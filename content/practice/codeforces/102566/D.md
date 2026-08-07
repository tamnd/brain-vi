---
title: "CF 102566D - Chính phủ"
description: "Có N dự án và M thành phố. Mỗi dự án phải được thực hiện theo đúng một trong hai cách có thể. Tùy chọn đầu tiên được coi là vô hại, trong khi tùy chọn thứ hai có hại. Mỗi lựa chọn đóng góp một số tiền nhất định cho mỗi thành phố."
date: "2026-08-07T21:33:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102566
codeforces_index: "D"
codeforces_contest_name: "AGM 2020, Qualification Round"
rating: 0
weight: 102566
solve_time_s: 54
verified: true
draft: false
---

[CF 102566D - Chính phủ](https://codeforces.com/problemset/problem/102566/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

có`N`dự án và`M`các thành phố. Mỗi dự án phải được thực hiện theo đúng một trong hai cách có thể. Tùy chọn đầu tiên được coi là vô hại, trong khi tùy chọn thứ hai có hại. Mỗi lựa chọn đóng góp một số tiền nhất định cho mỗi thành phố. 

Mục tiêu là chọn một phương án cho mỗi dự án sao cho tổng đóng góp ở mỗi thành phố phù hợp chính xác với ngân sách yêu cầu. Trong số tất cả các lựa chọn hợp lệ, chúng ta cần số lượng dự án nhỏ nhất có thể trong đó phương án có hại được chọn. Nếu không có lựa chọn dự án nào có thể đáp ứng được toàn bộ ngân sách thành phố thì câu trả lời là`impossible`. 

Các giá trị của`N`Và`M`cả hai đều có nhiều nhất là 30. Điều này ngay lập tức loại trừ việc thử tất cả các lựa chọn có thể có của dự án, vì mỗi dự án có hai trạng thái và tổng số cấu hình là`2^N`. Với`N = 30`, tức là hơn một tỷ khả năng, vượt xa những gì có thể kiểm tra trong thời hạn. kích thước`M`cũng đủ nhỏ để việc lưu trữ và so sánh các vectơ có độ dài 30 là thực tế, vì vậy giải pháp nên tập trung vào việc giảm số lượng kết hợp thay vì giảm kích thước vectơ. 

Một cách hữu ích để suy nghĩ về vấn đề là bắt đầu với sự lựa chọn vô hại cho mọi dự án. Điều này đưa ra một vectơ chi tiêu ban đầu cố định. Đối với mọi dự án, việc chuyển từ vô hại sang có hại sẽ tạo ra sự khác biệt. Nhiệm vụ trở thành tìm một tập hợp con của các vectơ sai phân này có tổng chính xác bằng số còn thiếu, đồng thời giảm thiểu số lượng vectơ đã chọn. 

Có một số trường hợp nguy hiểm có thể phá vỡ các giải pháp đơn giản hơn. Một giải pháp chỉ kiểm tra xem vectơ mục tiêu có tồn tại hay không nhưng không lưu trữ số lượng lựa chọn có hại tối thiểu có thể trả về câu trả lời hợp lệ nhưng không tối ưu. Ví dụ:```
N = 2, M = 1
budget = [2]
project 1: (0, 1)
project 2: (0, 2)
```Việc chọn phương án có hại của dự án 2 sẽ đạt đến ngân sách với một lựa chọn có hại, trong khi việc chọn cả hai phương án có hại cũng đạt đến ngân sách với hai lựa chọn có hại. Việc kiểm tra sự tồn tại bất cẩn cũng có thể trả về. 

Một vấn đề khác là quên rằng một lựa chọn có hại có thể có chi phí nhỏ hơn so với lựa chọn vô hại. Vectơ khác biệt không phải lúc nào cũng dương. Ví dụ:```
N = 1, M = 1
budget = [0]
project 1: (5, 0)
```Câu trả lời đúng là`1`, bởi vì việc chọn phương án có hại sẽ giảm mức chi tiêu đi 5 lần so với lựa chọn vô hại. Việc coi những lựa chọn có hại chỉ là sự gia tăng tích cực sẽ thất bại ở đây. 

Trường hợp thứ ba là giải pháp vô hại cơ bản có thể đã đáp ứng được mọi ngân sách của thành phố:```
N = 1, M = 1
budget = [5]
project 1: (5, 7)
```Câu trả lời là`0`. Bất kỳ thuật toán nào ép buộc ít nhất một lựa chọn có hại sẽ tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi phương án có thể được phân công cho các dự án. Đối với mỗi`2^N`nhiệm vụ, chúng tôi tính toán kết quả chi tiêu ở tất cả các thành phố, kiểm tra xem nó có phù hợp với ngân sách hay không và giữ lại số lượng lựa chọn có hại nhỏ nhất. Cách tiếp cận này đúng vì nó xem xét mọi quyết định có thể, nhưng trường hợp xấu nhất đòi hỏi`2^30`nhiệm vụ, tức là khoảng 1,07 tỷ tiểu bang. Ngay cả khi mỗi trạng thái được xử lý rất nhanh thì điều này vẫn quá chậm. 

Cấu trúc quan trọng là số lượng dự án chỉ có 30. Đây là phạm vi điển hình mà việc gặp nhau ở giữa rất hữu ích. Thay vì xử lý tất cả 30 quyết định cùng nhau, chúng tôi chia các dự án thành hai nhóm khoảng 15. Mỗi nửa chỉ có`2^15 = 32768`những lựa chọn có thể, có thể quản lý được. 

Những lựa chọn vô hại được sử dụng làm điểm khởi đầu. Đối với mỗi dự án, chúng tôi tính toán sự khác biệt giữa việc chọn có hại và chọn vô hại. Sau đó, một tập hợp con các dự án đại diện cho toàn bộ sự thay đổi gây ra bởi việc lựa chọn các phương án có hại cho các dự án đó. 

Trong nửa đầu, chúng tôi liệt kê mọi tập hợp con và lưu trữ vectơ sai phân kết quả cùng với số lượng các lựa chọn có hại được sử dụng. Đối với nửa thứ hai, chúng tôi liệt kê mọi tập hợp con và tìm kiếm vectơ bổ sung từ nửa đầu. Nếu tổng số thay đổi cần thiết là`target`, và nửa sau đóng góp`s`, thì nửa đầu phải đóng góp`target - s`. 

Số lượng có hại tối thiểu được tìm thấy bằng cách kết hợp trận đấu hay nhất của hiệp một với mọi khả năng có thể xảy ra của hiệp hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N * M) | O(M) | Quá chậm | 
| Gặp nhau ở giữa | O(2^(N/2) * M) | O(2^(N/2) * M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số tiền đóng góp nếu mỗi dự án sử dụng phương án vô hại của nó. Đối với mọi dự án, hãy tính toán vectơ được thêm vào khi thay đổi dự án đó thành tùy chọn có hại. Câu trả lời cuối cùng chỉ phụ thuộc vào vectơ sai phân nào được chọn. 
2. Tính vectơ chênh lệch mục tiêu bằng cách trừ đi tổng số vô hại từ ngân sách yêu cầu. Nếu không thể tạo vectơ này từ những khác biệt của dự án thì không có lựa chọn hợp lệ nào tồn tại. 
3. Chia các dự án thành hai nhóm. Nhóm đầu tiên chứa khoảng một nửa số dự án và nhóm thứ hai chứa phần còn lại. Điều này làm giảm việc tìm kiếm theo cấp số nhân từ`2^30`khả năng cho hai tìm kiếm về`2^15`khả năng. 
4. Liệt kê mọi tập hợp con của nhóm đầu tiên. Đối với mỗi tập hợp con, hãy tính tổng các vectơ sai phân của nó và số lựa chọn có hại mà nó chứa. Lưu trữ số lượng có hại tối thiểu cho mỗi vectơ kết quả. Chỉ giữ mức tối thiểu là đủ vì mọi sự kết hợp sau này chỉ quan tâm đến cách rẻ nhất để tạo ra vectơ đó. 
5. Liệt kê mọi tập hợp con của nhóm thứ hai. Đối với mỗi tập hợp con, hãy tính vectơ sai phân và số lượng có hại của nó. Vectơ bị thiếu cần có trong nhóm đầu tiên là vectơ mục tiêu trừ đi vectơ nhóm thứ hai hiện tại. 
6. Tra cứu vectơ nhóm đầu tiên được yêu cầu trong bản đồ được lưu trữ. Nếu nó tồn tại, hãy kết hợp hai số lượng có hại và cập nhật câu trả lời. 
7. Nếu không có cặp tập con nào tạo ra vectơ đích, hãy in`impossible`. Nếu không hãy in số lượng có hại nhỏ nhất được tìm thấy. 

Tại sao nó hoạt động: 

Mọi lựa chọn có thể có của các dự án có hại có thể được chia thành các dự án được chọn từ nửa đầu và các dự án được chọn từ nửa sau. Việc liệt kê cả hai nửa sẽ xem xét mọi sự phân chia như vậy. Đối với lựa chọn cố định ở nửa sau, việc tra cứu sẽ hỏi chính xác liệu nửa đầu tiên có thể tạo ra thay đổi cần thiết còn lại hay không. Vì mỗi vectơ nửa đầu được lưu trữ chỉ giữ lại số lượng có hại tối thiểu nên luôn sử dụng sự kết hợp tốt nhất có thể có cho vectơ đó. Do đó, mọi giải pháp hợp lệ đều được xem xét và số lượng lựa chọn có hại tối thiểu được bảo tồn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, m, budget, projects):
    base = [0] * m
    diff = []

    for harmful, harmless in projects:
        for j in range(m):
            base[j] += harmless[j]
        diff.append([harmful[j] - harmless[j] for j in range(m)])

    target = tuple(budget[j] - base[j] for j in range(m))

    first = diff[:n // 2]
    second = diff[n // 2:]

    def enumerate_half(arr):
        result = {}
        length = len(arr)
        total = 1 << length

        sums = [(0,) * m]
        counts = [0]

        for mask in range(1, total):
            bit = mask & -mask
            idx = bit.bit_length() - 1
            previous = mask ^ bit
            old = sums[previous]

            cur = tuple(old[j] + arr[idx][j] for j in range(m))
            sums.append(cur)
            counts.append(counts[previous] + 1)

            if cur not in result or counts[-1] < result[cur]:
                result[cur] = counts[-1]

        return result

    left = enumerate_half(first)

    answer = n + 1
    right_len = len(second)
    total = 1 << right_len
    sums = [(0,) * m]
    counts = [0]

    for mask in range(total):
        if mask != 0:
            bit = mask & -mask
            idx = bit.bit_length() - 1
            previous = mask ^ bit
            old = sums[previous]
            cur = tuple(old[j] + second[idx][j] for j in range(m))
            sums.append(cur)
            counts.append(counts[previous] + 1)
        else:
            cur = sums[0]

        need = tuple(target[j] - cur[j] for j in range(m))
        if need in left:
            answer = min(answer, counts[mask] + left[need])

    return "impossible" if answer == n + 1 else str(answer)

def main():
    data = sys.stdin.buffer.read().split()
    if not data:
        return

    it = iter(data)
    t = int(next(it))
    ans = []

    for _ in range(t):
        n = int(next(it))
        m = int(next(it))

        budget = [int(next(it)) for _ in range(m)]

        projects = []
        for _ in range(n):
            harmless = []
            harmful = []
            for _ in range(m):
                x = int(next(it))
                y = int(next(it))
                harmless.append(x)
                harmful.append(y)
            projects.append((harmful, harmless))

        ans.append(solve_case(n, m, budget, projects))

    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Phần đầu tiên của quá trình triển khai xây dựng đường cơ sở vô hại và các vectơ sai phân. Đường cơ sở thể hiện số tiền đã chi tiêu trước khi xem xét bất kỳ lựa chọn có hại nào, trong khi mỗi vectơ khác biệt mô tả tác động chính xác của việc khiến một dự án trở nên có hại. 

các`enumerate_half`Hàm thực hiện việc đáp ứng ở phần liệt kê ở giữa cho một bên. Mảng`sums`lưu trữ tổng tập hợp con được tính toán trước đó để tạo tập hợp con mới chỉ yêu cầu thêm một dự án thay vì tính toán lại toàn bộ tập hợp con. Từ điển lưu trữ số lượng có hại nhỏ nhất cho mỗi vectơ vì nhiều tập hợp con có thể tạo ra cùng một thay đổi chi tiêu. 

Nửa sau được liệt kê trong khi tìm kiếm phần bổ sung trong từ điển nửa đầu. Vectơ yêu cầu được tính như`target - current_second_half`. Một trận đấu có nghĩa là hai nửa cùng nhau tạo ra chính xác phần điều chỉnh chi tiêu còn thiếu. 

Số nguyên Python không bị tràn nên mối quan tâm duy nhất là việc sử dụng bộ nhớ. Nhiều nhất`2^15`vectơ có độ dài 30 được lưu trữ, điều này có thể chấp nhận được. Việc biểu diễn bộ dữ liệu được sử dụng vì bộ dữ liệu có thể là khóa từ điển. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp nhỏ này:```
1
2 1
2
0 0
0 2
```Dự án đầu tiên thay đổi tổng số bằng`0`nếu gây hại. Thứ hai thay đổi nó bằng cách`2`. Sự khác biệt mục tiêu so với đường cơ sở vô hại là`2`. 

| Bước | Tập hợp con hiện tại | Vectơ khác biệt | Tính có hại | Kết quả | 
| --- | --- | --- | --- | --- | 
| Nửa đầu | không | (0) | 0 | được lưu trữ | 
| Nửa đầu | dự án 1 | (0) | 1 | bỏ qua vì tệ hơn | 
| Hiệp hai | không | (0) | 0 | nhu cầu (2), không tìm thấy | 
| Hiệp hai | dự án 2 | (2) | 1 | nhu cầu (0), được tìm thấy | 

Thuật toán kết hợp dự án thứ hai với tập con nửa đầu trống, đưa ra một lựa chọn có hại. Ví dụ này chứng minh tại sao các vectơ trùng lặp phải giữ số lượng tối thiểu. 

Một ví dụ thứ hai:```
1
1 1
5
5 5
```Đường cơ sở vô hại đã bằng ngân sách. 

| Bước | Tập hợp con hiện tại | Vectơ khác biệt | Tính có hại | Kết quả | 
| --- | --- | --- | --- | --- | 
| Tính toán mục tiêu | không | (0) | 0 | bắt buộc | 
| Nửa đầu | trống | (0) | 0 | được lưu trữ | 
| Hiệp hai | trống | (0) | 0 | trận đấu | 

Lựa chọn trống được chấp nhận, đưa ra câu trả lời`0`. Điều này xác nhận rằng thuật toán không ép buộc những lựa chọn có hại không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^(N/2) * M) | Mỗi nửa liệt kê nhiều nhất`2^15`tập hợp con và mỗi thao tác vectơ chạm vào`M`các thành phố. | 
| Không gian | O(2^(N/2) * M) | Từ điển nửa đầu lưu trữ tất cả các vectơ sai phân tập hợp con duy nhất. | 

Với`N <= 30`Và`M <= 30`, thuật toán xử lý tối đa khoảng 32768 tập con mỗi nửa. Điều này phù hợp thoải mái trong giới hạn, ngay cả đối với nhiều trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    main()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

assert run("""1
2 1
2
0 0
0 2
""") == "1\n", "basic possible case"

assert run("""1
1 1
0
5 0
""") == "1\n", "harmful option decreases cost"

assert run("""1
1 1
5
5 7
""") == "0\n", "already satisfied"

assert run("""1
2 2
3 3
1 1 0 0
1 0 0 1
""") == "impossible\n", "unreachable vector"

assert run("""1
30 1
30
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
1 1
""") == "0\n", "maximum number of projects with equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp cơ bản có thể xảy ra |`1`| Gặp nhau bình thường ở giữa trận đấu | 
| Giảm chi phí có hại |`1`| Vectơ sai phân có thể chứa giá trị âm | 
| Đã hài lòng |`0`| Tập hợp con trống là câu trả lời hợp lệ | 
| Vectơ không thể truy cập |`impossible`| Phát hiện đúng không có giải pháp | 
| Ba mươi dự án bằng nhau |`0`| Xử lý số lượng dự án tối đa | 

## Vỏ cạnh 

Đối với trường hợp một số tập hợp con tạo ra cùng một vectơ khác biệt, bản cập nhật từ điển chỉ giữ lại số lượng có hại nhỏ nhất. Ví dụ:```
1
2 1
2
0 0
0 2
```Nửa đầu có thể tạo ra cùng một vectơ từ các lựa chọn khác nhau. Chỉ lưu trữ số lượng tốt nhất sẽ ngăn cản sự kết hợp sau này sử dụng cách biểu diễn đắt tiền hơn. 

Đối với những khác biệt tiêu cực, thuật toán không bao giờ cho rằng những lựa chọn có hại sẽ làm tăng chi tiêu. TRONG:```
1
1 1
0
5 0
```đường cơ sở vô hại là 5 và chênh lệch mục tiêu là -5. Vectơ sai phân được lưu trữ cũng là -5, do đó lựa chọn có hại được tìm thấy chính xác. 

Đối với đường cơ sở đã được thỏa mãn:```
1
1 1
5
5 7
```vectơ mục tiêu bằng 0. Tập hợp con trống xuất hiện trong quá trình liệt kê và khớp ngay lập tức, tạo ra số lượng có hại tối thiểu có thể bằng 0.
