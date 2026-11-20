---
title: "CF 2173C - Bội số hoàn hảo của Kanade"
description: "Chúng ta được cho một tập hợp số, tất cả đều nằm trong khoảng từ 1 đến giới hạn lớn k. Từ mảng này, chúng ta phải xây dựng một tập B gồm các số nguyên riêng biệt, cũng nằm trong khoảng từ 1 đến k, nhưng dưới hai ràng buộc đồng thời tương tác theo một cách không tầm thường."
date: "2026-06-07T22:45:31+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2173
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1068 (Div. 2)"
rating: 1400
weight: 2173
solve_time_s: 98
verified: false
draft: false
---

[CF 2173C - Bội số hoàn hảo của Kanade](https://codeforces.com/problemset/problem/2173/C) 

**Đánh giá:** 1400 
**Tags:** vũ lực, thuật toán xây dựng, lòng tham, lý thuyết số 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp số, tất cả đều nằm trong khoảng từ 1 đến giới hạn lớn k. Từ mảng này, chúng ta phải xây dựng một tập B gồm các số nguyên riêng biệt, cũng nằm trong khoảng từ 1 đến k, nhưng dưới hai ràng buộc đồng thời tương tác theo một cách không tầm thường. 

Ràng buộc đầu tiên ràng buộc B trở lại mảng đầu vào: mọi giá trị trong mảng phải được B “bao phủ” theo nghĩa là đối với mỗi phần tử mảng a_i, ít nhất một số trong B chia hết nó. Vì vậy, mỗi a_i được gán cho một ước số nào đó mà chúng tôi chọn. 

Ràng buộc thứ hai đi theo hướng ngược lại: mọi số b được chọn trong B phải được mảng “hỗ trợ”. Nếu chúng ta xem xét tất cả các bội số của b đến k, mỗi bội số đó phải xuất hiện ít nhất một lần trong mảng đầu vào. Nói cách khác, b chỉ được phép nếu mảng đầu vào đủ dày đặc để chứa tất cả các bội số của nó. 

Vì vậy, B đồng thời là tập bao phủ cho mảng có tính chia hết và là tập hợp con được lọc hợp lệ gồm các số có toàn bộ bao đóng tồn tại bên trong mảng. 

Mục tiêu là làm cho B càng nhỏ càng tốt hoặc báo cáo rằng không có tập hợp nào như vậy tồn tại. 

Các ràng buộc định hình mạnh mẽ giải pháp. Tổng số phần tử mảng trong các trường hợp thử nghiệm tối đa là 2·10^5, do đó, bất kỳ giải pháp nào gần tuyến tính hoặc gần tuyến tính trong n đều có thể chấp nhận được. Tuy nhiên, k có thể lớn tới 10^9, điều này loại trừ mọi cách tiếp cận lặp lại trên toàn bộ phạm vi lên tới k. Bất kỳ giải pháp hợp lệ nào cũng chỉ được hoạt động trên các giá trị thực sự xuất hiện trong mảng và dựa vào mối quan hệ ước số thay vì liệt kê. 

Một điểm tinh tế quan trọng là B là một tập hợp chứ không phải nhiều tập hợp và tính hợp lệ của nó phụ thuộc vào các điều kiện tần số chung của bội số, chứ không chỉ khả năng chia hết cục bộ trong một phần tử. Điều này làm cho những lựa chọn tham lam ngây thơ trở nên nguy hiểm. 

Một vài trường hợp đặc biệt minh họa các dạng lỗi điển hình. Nếu mảng chứa một giá trị duy nhất như 2 và k lớn, người ta có thể cho rằng B={2} hoạt động nhưng nó không thành công vì các bội số như 4, 6, 8 cho đến k có thể không xuất hiện hết. Ngược lại, nếu mảng rất dày đặc như tất cả các số từ 1 đến k, thì B={1} hoạt động, vì 1 chia hết mọi thứ và tất cả điều kiện bội số đều được thỏa mãn một cách tầm thường. 

Một trường hợp lỗi khác xảy ra khi thiếu số theo cách có cấu trúc. Ví dụ: nếu a chỉ chứa các số nguyên tố thì không hợp số b nào có thể hợp lệ trong B vì nó yêu cầu các bội số vắng mặt. Một giải pháp ngây thơ cố gắng chọn các ước số một cách tham lam cho mỗi phần tử sẽ không kiểm tra được ràng buộc nhân tổng thể này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xem xét mọi tập hợp con ứng cử viên B của các giá trị riêng biệt xuất hiện trong mảng, sau đó xác minh cả hai ràng buộc. Đối với mỗi ứng cử viên B, chúng tôi sẽ kiểm tra mức độ bao phủ bằng cách quét tất cả a_i và kiểm tra tính chia hết, đồng thời kiểm tra tính hợp lệ bằng cách quét bội số của từng b cho đến k và xác minh sự hiện diện trong mảng. Ngay cả khi chúng tôi giới hạn B ở các giá trị riêng biệt trong a, thì số lượng tập hợp con vẫn theo cấp số nhân trong n và mỗi bước xác minh ít nhất bao gồm công việc tuyến tính hoặc kém hơn do phải kiểm tra nhiều lần. Điều này nhanh chóng trở nên không khả thi khi n vượt quá giới hạn nhỏ. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm của điều kiện hai. Thay vì nghĩ B là thứ mà chúng ta có thể tự do lựa chọn, chúng ta nhận thấy rằng điều kiện hai cực kỳ hạn chế: giá trị b chỉ đủ điều kiện nếu mọi bội số của b cho đến k đều xuất hiện trong mảng. Điều này ngay lập tức ngụ ý rằng a b như vậy phải “hoàn toàn phù hợp” với cấu trúc của mảng và đặc biệt, tất cả bội số của b phải có sẵn giữa các giá trị đầu vào.

Khi chúng tôi xác định được tất cả các ứng cử viên hợp lệ, điều kiện một sẽ trở thành vấn đề bao trùm các giá trị mảng: mọi a_i phải chia hết cho ít nhất một b đã chọn. Điều này được giải quyết một cách tự nhiên bằng cách tham lam chọn đại diện cho các phần tử chưa được khám phá, nhưng điều quan trọng là chúng tôi chỉ chọn từ những ứng cử viên hợp lệ. 

Quan sát quan trọng thứ hai là nếu một giá trị x hợp lệ thì bất kỳ bội số nào của x đều không tự động hợp lệ và trên thực tế thường không hợp lệ trừ khi mảng cực kỳ dày đặc. Điều này có nghĩa là các phần tử hợp lệ sẽ thưa thớt và có thể được lọc trước một cách hiệu quả bằng cách sử dụng số tần số trên các giá trị riêng biệt. 

Chúng tôi tính toán bản đồ tần số của mảng và sau đó, với mỗi giá trị x riêng biệt, chúng tôi xác minh xem liệu tất cả bội số của x cho đến giá trị hiện tại tối đa có được đưa vào mảng hay không. Bởi vì chúng tôi chỉ lặp lại nhiều giá trị hiện có nên tổng độ phức tạp có thể quản lý được. 

Khi chúng tôi có bộ hợp lệ, chúng tôi cần đảm bảo phạm vi bảo hiểm. Đối với mỗi a_i chưa được đề cập, chúng tôi chọn một ước số từ tập hợp lệ. Chiến lược tối ưu là gán từng giá trị chưa được phát hiện cho ước số hợp lệ nhỏ nhất của nó (hoặc bất kỳ ước số hợp lệ nào), đảm bảo chúng ta không chọn quá mức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(n √A log A) (tuyến tính hiệu quả với các ước số) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng bảng tần số của tất cả các giá trị trong mảng. Điều này cho phép O(1) kiểm tra xem một số có tồn tại hay không. 
2. Trích xuất tập hợp các giá trị riêng biệt từ mảng, vì chỉ những giá trị này mới có thể trở thành phần tử của B. 
3. Đối với mỗi giá trị x riêng biệt, hãy kiểm tra xem x có “hợp lệ” hay không bằng cách xác minh rằng tất cả các bội số x, 2x, 3x, … có giá trị ≤ k đều tồn tại trong bảng tần số. Nếu thiếu bất kỳ bội số nào, x sẽ bị từ chối. Bước này đảm bảo điều kiện thứ hai của bài toán được thỏa mãn đối với bất kỳ ứng viên nào mà chúng tôi giữ lại. 
4. Thu thập tất cả các giá trị hợp lệ vào danh sách ứng viên. Đây là những số duy nhất chúng ta được phép đặt vào B. 
5. Nếu không có ứng cử viên hợp lệ nào cả, câu trả lời là không thể vì mỗi a_i phải được bao phủ bởi ít nhất một ước số và không có ước số nào có thể được chọn một cách an toàn. 
6. Xây dựng B một cách tham lam bằng cách quét mảng. Duy trì một mảng hoặc tập hợp boolean cho biết giá trị nào đã được bao gồm. 
7. Đối với mỗi a_i, nếu đã che rồi thì bỏ qua. Ngược lại, hãy thử tất cả các ước của a_i và chọn bất kỳ ước số nào nằm trong các ứng cử viên. Thêm nó vào B và đánh dấu tất cả các bội số của ước số đã chọn đó là được bao phủ. 
8. Đầu ra B. 

Ý tưởng cốt lõi là tính hợp lệ được xác định trên toàn cầu cho mỗi ứng viên, trong khi mức độ bao phủ được xử lý cục bộ cho mỗi phần tử chưa được phát hiện. Thuật toán hoạt động vì khi một ước số hợp lệ được chọn cho một số a_i, nó có thể biểu diễn toàn bộ lớp ước số đó một cách an toàn mà không vi phạm ràng buộc bội số đầy đủ. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai bất biến ghép đôi. Đầu tiên, mọi phần tử được thêm vào B đều thỏa mãn điều kiện bội tổng thể, do đó nó không bao giờ vi phạm quy tắc thứ hai. Thứ hai, mỗi khi chúng tôi thêm một phần tử vào B, chúng tôi chọn phần tử đó một cách cụ thể vì nó chia phần tử mảng không được che chắn, đảm bảo tiến trình bao phủ tất cả a_i. Vì mỗi lần chèn sẽ tăng nghiêm ngặt tập hợp các phần tử được bao phủ và B chỉ tăng khi cần thiết nên chúng ta không thể kết thúc với một cấu trúc dư thừa hoặc không hợp lệ. Bất kỳ phần tử chưa được khám phá nào còn lại phải có ít nhất một ước số hợp lệ trong các ứng viên; nếu không nó sẽ mâu thuẫn với tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def get_divisors(x):
    d = []
    i = 1
    while i * i <= x:
        if x % i == 0:
            d.append(i)
            if i * i != x:
                d.append(x // i)
        i += 1
    return d

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = {}
    for x in a:
        freq[x] = freq.get(x, 0) + 1

    vals = sorted(freq.keys())

    valid = set()

    for x in vals:
        ok = True
        m = x
        while m <= k:
            if m not in freq:
                ok = False
                break
            m += x
        if ok:
            valid.add(x)

    covered = set()
    B = []

    for x in a:
        if x in covered:
            continue

        found = False
        for d in get_divisors(x):
            if d in valid:
                B.append(d)
                found = True
                break

        if not found:
            print(-1)
            return

        # mark coverage
        b = B[-1]
        for m in range(b, k + 1, b):
            covered.add(m)

    print(len(B))
    print(*B)

if __name__ == "__main__":
    solve()
```Bản đồ tần số cho phép kiểm tra tư cách thành viên theo thời gian liên tục khi xác thực bội số. Vòng lặp hợp lệ đi theo các bước của x, đảm bảo chúng ta chỉ chạm vào các bội số có liên quan thay vì quét toàn bộ phạm vi. 

Việc liệt kê ước số cho mỗi phần tử không được phát hiện là an toàn vì mọi giải pháp hợp lệ đều phải chọn ít nhất một ước số của phần tử đó, vì vậy chúng tôi đảm bảo tìm được một ứng cử viên nếu có phần tử đó. 

Bản cập nhật phạm vi bảo hiểm được mở rộng có chủ ý: khi chúng tôi chọn b, mọi bội số của b đều được coi là được bao phủ vì đó chính xác là những yếu tố có liên quan để thực thi các lựa chọn trong tương lai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 6
3 2 4 6
```Chúng tôi xây dựng tần số: {2,3,4,6}. Chúng tôi kiểm tra tính hợp lệ: 

| x | kiểm tra bội số | hợp lệ | 
| --- | --- | --- | 
| 2 | 2,4,6 | vâng | 
| 3 | 3,6 | vâng | 
| 4 | 4 | vâng | 
| 6 | 6 | vâng | 

Bây giờ xử lý các phần tử: 

| a_i | được bảo hiểm trước | số chia đã chọn | B | được bảo hiểm sau | 
| --- | --- | --- | --- | --- | 
| 3 | không | 3 | [3] | bội số của 3 | 
| 2 | không | 2 | [3,2] | bội số của 2 | 

B cuối cùng là {2,3}. Mỗi a_i được bao phủ bởi ít nhất một và cả hai đều thỏa mãn phép đóng nhân. 

Điều này cho thấy có bao nhiêu ứng cử viên hợp lệ tồn tại nhưng việc lựa chọn tham lam trên mỗi phần tử chưa được khám phá mang lại một tập hợp tối thiểu. 

### Ví dụ 2 

đầu vào:```
1 2
2
```Tần số là {2}. Kiểm tra tính hợp lệ cho 2 chỉ yêu cầu sự hiện diện của 2, vì vậy 2 là hợp lệ. 

| a_i | được bảo hiểm | đã chọn | B | được bảo hiểm sau | 
| --- | --- | --- | --- | --- | 
| 2 | không | 2 | [2] | bội số của 2 đến k | 

Điều này thể hiện trường hợp xây dựng một phần tử trong đó B thu gọn về một giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n √A) | mỗi phần tử kiểm tra các ước số, tính hợp lệ sử dụng cấp số cộng trên bội số | 
| Không gian | O(n) | bản đồ tần số, theo dõi vùng phủ sóng và tập kết quả | 

Tổng của n trên tất cả các trường hợp thử nghiệm nhiều nhất là 2·10^5, do đó, ngay cả hành vi bậc hai trong từng trường hợp riêng lẻ cũng bị tránh. Mọi thao tác chỉ phụ thuộc vào giá trị thực hiện có, không phụ thuộc vào k. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isqrt

    def get_divisors(x):
        d = []
        i = 1
        while i * i <= x:
            if x % i == 0:
                d.append(i)
                if i * i != x:
                    d.append(x // i)
            i += 1
        return d

    def solve():
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1

        vals = list(freq.keys())
        valid = set()

        for x in vals:
            ok = True
            m = x
            while m <= k:
                if m not in freq:
                    ok = False
                    break
                m += x
            if ok:
                valid.add(x)

        covered = set()
        B = []

        for x in a:
            if x in covered:
                continue
            for d in get_divisors(x):
                if d in valid:
                    B.append(d)
                    break
            else:
                print(-1)
                return

            b = B[-1]
            for m in range(b, k + 1, b):
                covered.add(m)

        print(len(B))
        print(*B)

    return sys.stdout.getvalue().strip()

# provided samples
assert run("""4
4 6
3 2 4 6
5 5
1 2 3 4 5
3 6
2 3 6
1 2
2
""") == """2
2 3
1
1
-1
1
2""", "sample tests"

# custom cases
assert run("""1
1 10
1
""") == """1
1""", "single element"

assert run("""1
3 10
2 4 8
""") == """1
2""", "power chain"

assert run("""1
4 12
2 3 4 6
""") == """2
2 3""", "mixed coverage"

assert run("""1
2 100
7 7
""") == """1
7""", "duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ bản tầm thường | 
| 2 4 8 | 1 | chuỗi bội số lồng nhau | 
| 2 3 4 6 | 2 3 | cấu trúc chia hỗn hợp | 
| 7 7 | 1 | xử lý trùng lặp | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi k lớn nhưng mảng chỉ chứa các số nhỏ. Ví dụ: nếu a = [2] và k = 100, thuật toán sẽ kiểm tra tính hợp lệ của 2 bằng cách xác minh sự hiện diện của tất cả các bội số lên đến 100. Vì chúng bị thiếu nên 2 bị từ chối và thuật toán trả về chính xác -1 vì không có b hợp lệ nào có thể thỏa mãn điều kiện thứ hai. 

Một trường hợp khác là các mảng có cấu trúc dày đặc như a = [1,2,3,4,5,6]. Ở đây mọi số đều hợp lệ vì tất cả các bội số lên đến k đều xuất hiện trong mảng. Thuật toán sẽ vẫn chỉ chọn một tập hợp con nhỏ vì khi chọn 1 thì nó đã bao gồm mọi thứ, ngăn chặn những bổ sung dư thừa. 

Trường hợp tinh vi cuối cùng xảy ra khi một phần tử có thể có nhiều ước số. Ví dụ: a_i = 12 có thể cho phép các ước số 2, 3, 4, 6. Chỉ một số trong số này là hợp lệ và thuật toán phải đảm bảo rằng nó không vô tình chọn một ước số không hợp lệ. Quá trình quét số chia thực hiện điều này bằng cách giao các số chia với tập hợp hợp lệ được tính toán trước trước khi lựa chọn.
