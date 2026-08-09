---
title: "CF 102460J - Máy Điều Khiển Tự Động"
description: "Mỗi máy có một tập hợp các tính năng phải được kiểm tra. Tập dữ liệu thử nghiệm có thể phát hiện một số tập hợp con của các tính năng đó, được biểu thị bằng chuỗi nhị phân có độ dài (n). Chọn một số bộ dữ liệu có nghĩa là kết hợp tất cả các tính năng được phát hiện bởi các bộ dữ liệu đó."
date: "2026-08-08T10:18:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102460
codeforces_index: "J"
codeforces_contest_name: "2019-2020 ICPC Asia Taipei-Hsinchu Regional Contest"
rating: 0
weight: 102460
solve_time_s: 310
verified: true
draft: false
---

[CF 102460J - Máy điều khiển tự động](https://codeforces.com/problemset/problem/102460/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi máy có một tập hợp các tính năng phải được kiểm tra. Tập dữ liệu thử nghiệm có thể phát hiện một số tập hợp con của các tính năng đó, được biểu thị bằng chuỗi nhị phân có độ dài\(n\). Chọn một số bộ dữ liệu có nghĩa là kết hợp tất cả các tính năng được phát hiện bởi các bộ dữ liệu đó. Nhiệm vụ là tìm số lượng tập dữ liệu nhỏ nhất có liên kết chứa mọi tính năng. Nếu một số tính năng không bao giờ được phát hiện bởi bất kỳ tập dữ liệu nào, câu trả lời là\(-1\). 

Các ràng buộc được cố tình không đối xứng. Có thể có tới 500 đặc điểm, quá nhiều cho một tập hợp con về các đặc điểm vì\(2^{500}\)là hoàn toàn không khả thi. Mặt khác, có tối đa 15 bộ dữ liệu thử nghiệm. Điều đó làm cho\(2^m\), nhiều nhất\(2^{15}=32768\), đủ nhỏ để liệt kê cho mọi máy. Số lượng máy cũng nhiều nhất là 10, do đó, thậm chí có thể dễ dàng quản lý việc xử lý tất cả các tập hợp con cho mỗi máy. 

Việc thực hiện bất cẩn vẫn có thể thất bại ở một số ranh giới. Hãy xem xét một tính năng duy nhất với một tập dữ liệu không thể kiểm tra nó:```text
1
1 1
0
```Câu trả lời đúng là`-1`. Một giải pháp khởi tạo câu trả lời về 0 và chỉ giảm nó khi tìm thấy giải pháp có thể in không chính xác số 0. 

Trường hợp ngược lại cũng hữu ích:```text
1
1 1
1
```Câu trả lời đúng là`1`, không phải bằng không. Tập hợp con trống không bao gồm đặc điểm nào, vì vậy nó không bao giờ được coi là một giải pháp hợp lệ. 

Bộ dữ liệu trùng lặp không mang lại bất kỳ lợi thế đặc biệt nào. Ví dụ:```text
1
4 3
1111
1111
1111
```Câu trả lời là`1`, bởi vì một tập dữ liệu đã bao gồm mọi thứ. Giải pháp tính các mẫu phạm vi bao phủ riêng biệt thay vì các tập dữ liệu đã chọn vẫn có thể giải quyết đúng trường hợp này, nhưng các thuật toán vô tình yêu cầu phạm vi bao phủ khác nhau từ mỗi tập dữ liệu đã chọn có thể thất bại. 

Cuối cùng, một tính năng chỉ có thể được đề cập bằng cách kết hợp nhiều bộ dữ liệu. Ví dụ:```text
1
3 2
100
011
```Câu trả lời là`2`. Việc chỉ kiểm tra xem một số tập dữ liệu riêng lẻ có bao gồm tất cả các tính năng hay không sẽ báo cáo sai`-1`. 

Mẫu chính thức chứa năm máy và có kết quả đầu ra`1, 2, 4, 3, -1`. Chuỗi nhị phân hoàn chỉnh có trong tuyên bố cuộc thi ban đầu. citturn2view0 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là xem xét mọi tập hợp con của\(m\)các tập dữ liệu thử nghiệm. Đối với mỗi tập hợp con, bộ sưu tập dữ liệu quét xuất hiện chính xác một lần trong số các tập hợp con.\(2^m\)tập hợp con. 

Nếu phạm vi bao phủ được biểu diễn một cách đơn giản dưới dạng mảng boolean thì việc xử lý một tập hợp con có thể mất \(O(mn)\) thời gian. Trong trường hợp xấu nhất điều này mang lại\[
O(2^m mn).
\]Với\(m=15\)Và\(n=500\), đại khái là vậy\(32768 \times 15 \times 500\), hoặc khoảng 246 triệu lượt kiểm tra tính năng cơ bản trên mỗi máy. Python sẽ có rất ít chỗ cho điều đó dưới giới hạn hai giây. 

Quan sát quan trọng là kích thước nhỏ là số lượng bộ dữ liệu chứ không phải số lượng tính năng. Chúng ta nên liệt kê các tập hợp con của bộ dữ liệu, nhưng chúng ta nên trình bày tập hợp các tính năng được đề cập một cách gọn gàng. 

Số nguyên Python là các bit có độ chính xác tùy ý. Chúng ta có thể biến mỗi chuỗi nhị phân thành một số nguyên, trong đó bit\(j\)đại diện cho dù tính năng\(j\)được che phủ. Việc kết hợp các tập dữ liệu sau đó chỉ đơn giản là theo bit OR. Sự kết hợp của 500 tính năng chỉ phù hợp với một số từ máy bên trong và Python thực hiện thao tác OR trong mã gốc được tối ưu hóa. 

Có một cải tiến hữu ích hơn. Khi xem xét một tập hợp con, hãy xóa một tập dữ liệu đã chọn và sử dụng lại phạm vi bao phủ của tập hợp con nhỏ hơn. Nếu như`mask`là tập hợp con hiện tại và`bit`là bit được đặt thấp nhất của nó, thì```text
coverage[mask] = coverage[mask without bit] | dataset[bit]
```Do đó, mỗi tập hợp con chỉ cần một phép toán OR số nguyên thay vì xây dựng lại liên kết của nó từ tất cả các tập dữ liệu đã chọn. 

Cách tiếp cận vũ phu có hiệu quả vì mọi lựa chọn có thể phải được xem xét, nhưng nó thất bại vì nó liên tục tính toán lại các hợp nhất từng phần giống nhau. Giá trị nhỏ của\(m\)hãy để chúng tôi giữ\(2^m\)không gian tìm kiếm, trong khi các tập hợp bit và cấu trúc tập hợp con tăng dần làm cho công việc trên mỗi tập hợp con trở nên nhỏ bé. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | \(O(2^m mn)\) | \(O(n)\) | Quá chậm trong Python | 
| Tối ưu | \(O(2^m \lceil n/w\rceil)\) | \(O(2^m \lceil n/w\rceil)\) | Đã chấp nhận | 

Đây\(w\)là kích thước từ máy được sử dụng nội bộ bởi biểu diễn bitset. Từ\(n\le500\), nó thực sự là một hằng số nhỏ cho bài toán này. 

## Hướng dẫn thuật toán 

1. Chuyển đổi chuỗi nhị phân của mọi tập dữ liệu thành số nguyên. Chút\(j\)được đặt chính xác khi tập dữ liệu\(j\)có thể kiểm tra tính năng tương ứng. Sử dụng số nguyên có nghĩa là việc kết hợp hai bộ tính năng được kiểm tra sẽ trở thành một OR theo bit đơn lẻ. 

2. Xây dựng`full = (1 << n) - 1`. Số nguyên này có tất cả\(n\)các bit tính năng được thiết lập, do đó một bộ sưu tập bao gồm mọi tính năng một cách chính xác khi số nguyên phạm vi phủ sóng của nó bằng`full`. 

3. Cấp phát một mảng`coverage`kích thước\(2^m\). Lối vào`coverage[mask]`lưu trữ tập hợp tất cả các tập dữ liệu được chọn bởi tập hợp con được đại diện bởi`mask`. Tập con trống có`coverage[0] = 0`. 

4. Liệt kê mọi tập con khác rỗng`mask`. Trích xuất bit được đặt thấp nhất của nó bằng`mask & -mask`. Vị trí của bit đó xác định một tập dữ liệu thuộc tập hợp con. 

5. Xóa tập dữ liệu đó khỏi tập hợp con để có được`previous = mask ^ bit`. Phạm vi bao phủ của tập hợp con hiện tại là`coverage[previous] | datasets[index]`. Đây là bước tái sử dụng trọng tâm vì tập hợp con nhỏ hơn đã được xử lý. 

6. Bất cứ khi nào phạm vi bảo hiểm kết quả bằng`full`, tính toán`mask.bit_count()`và sử dụng nó để cập nhật câu trả lời tối thiểu. Mọi tập hợp con đều được kiểm tra, do đó đảm bảo tìm thấy tập hợp con hợp lệ nhỏ nhất. 

7. Nếu không có tập con nào đạt tới`full`, in`-1`. Nếu không thì in số lượng tập dữ liệu được chọn nhỏ nhất được tìm thấy. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý một tập hợp con`mask`,`coverage[mask]`chính xác là sự kết hợp của tất cả các tập dữ liệu được chọn bởi`mask`. Điều này ban đầu giữ cho tập hợp con trống. Đối với bất kỳ tập hợp con không trống nào, việc xóa một tập dữ liệu đã chọn sẽ tạo ra một tập hợp con nhỏ hơn có phạm vi bao phủ chính xác và việc HOẶC tập dữ liệu đã xóa sẽ thêm chính xác các tính năng mà nó có thể kiểm tra. Do đó, mọi tập hợp con đều có phạm vi bao phủ chính xác. Vì thuật toán kiểm tra mọi tập hợp con có thể có và chấp nhận chính xác những tập hợp con có phạm vi bao phủ chứa mọi đặc điểm, nên số lượng tập hợp tối thiểu được ghi lại chính xác là số lượng tập dữ liệu tối thiểu được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    answers = []

    for _ in range(T):
        n, m = map(int, input().split())

        datasets = []
        for _ in range(m):
            datasets.append(int(input().strip(), 2))

        full = (1 << n) - 1
        total_masks = 1 << m

        coverage = [0] * total_masks
        answer = m + 1

        for mask in range(1, total_masks):
            bit = mask & -mask
            index = bit.bit_length() - 1
            previous = mask ^ bit

            coverage[mask] = coverage[previous] | datasets[index]

            if coverage[mask] == full:
                count = mask.bit_count()
                if count < answer:
                    answer = count

        answers.append(str(answer if answer <= m else -1))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Các chuỗi đầu vào được chuyển đổi với`int(string, 2)`. Ký tự ngoài cùng bên trái trở thành bit có thứ tự cao hơn, nhưng điều đó không quan trọng vì chúng tôi chỉ quan tâm liệu mọi vị trí có được bao phủ nhất quán trên tất cả các tập dữ liệu hay không.`full = (1 << n) - 1`tạo ra chính xác\(n\)một bit. Vì mọi tập dữ liệu đều có chính xác\(n\)các ký tự, không có bit không liên quan nào có thể xuất hiện phía trên vị trí\(n-1\). 

biểu hiện`mask & -mask`cô lập tập dữ liệu được chọn thấp nhất.`bit.bit_length() - 1`chuyển đổi bit bị cô lập đó thành chỉ mục tập dữ liệu dựa trên số 0 tương ứng.`mask ^ bit`loại bỏ nó, bởi vì bit đó được đảm bảo được đặt trong`mask`. 

Mảng`coverage`có\(2^m\)các mục, bao gồm cả tập hợp con trống. Bắt đầu đếm tại`1`tránh coi tập hợp con trống là một giải pháp. Từ\(n>0\), tập con trống không bao giờ có thể bao gồm`full`Dẫu sao thì. 

Số nguyên Python không bị tràn, do đó số lượng tính năng là 500 không yêu cầu xử lý số đặc biệt. Công việc tốn kém được thực hiện bằng các phép toán OR số nguyên thay vì các vòng lặp cấp Python trên các tính năng riêng lẻ. 

## Ví dụ đã hoạt động 

Mẫu chính thức bắt đầu với chiếc máy này:```text
3 3
100
011
111
```Có ba bộ dữ liệu và ba tính năng. Mẫu hoàn chỉnh mang lại cho chiếc máy này câu trả lời`1`. citturn2view0 

| Mặt nạ | Bộ dữ liệu được chọn | Bảo hiểm | Đếm | Tốt nhất | 
|---:|---|---|---:|---:| 
| 000 | không | 000 | 0 | không hợp lệ | 
| 001 | tập dữ liệu 1 | 100 | 1 | không hợp lệ | 
| 010 | tập dữ liệu 2 | 011 | 1 | không hợp lệ | 
| 011 | bộ dữ liệu 1, 2 | 111 | 2 | 2 | 
| 100 | tập dữ liệu 3 | 111 | 1 | 1 | 

Tập hợp con`011`chứng minh rằng cả ba tính năng đều có thể thực hiện được với hai bộ dữ liệu. Tập hợp con sau`100`cho thấy rằng chỉ riêng tập dữ liệu thứ ba đã bao gồm mọi thứ, do đó giá trị tối ưu giảm xuống còn`1`. 

Máy thứ năm trong mẫu chính thức là:```text
2 1
01
```Chỉ có một tập dữ liệu và nó chỉ kiểm tra tính năng thứ hai. Tính năng đầu tiên không thể kiểm tra được nên câu trả lời là`-1`. citturn2view0 

| Mặt nạ | Bộ dữ liệu được chọn | Bảo hiểm | Đếm | Tốt nhất | 
|---:|---|---|---:|---:| 
| 0 | không | 00 | 0 | không hợp lệ | 
| 1 | tập dữ liệu 1 | 01 | 1 | không hợp lệ | 

Không có tập hợp con nào tạo ra`11`, do đó thuật toán kết thúc với giá trị trọng điểm ban đầu và in ra`-1`. Điều này chứng tỏ tại sao điều không thể xảy ra phải được xử lý tách biệt khỏi việc tìm ra mức tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(T2^m\lceil n/w\rceil)\) | Mỗi trong số\(2^m\)các tập hợp con thực hiện một bit OR và đếm nhiều nhất một bit | 
| Không gian | \(O(2^m\lceil n/w\rceil)\) | Mảng bảo hiểm lưu trữ một bitset cho mỗi tập hợp con tập dữ liệu | 

Với\(m\le15\), có nhiều nhất 32768 tập con trên mỗi máy. Chỉ với 500 tính năng, mỗi bitset rất nhỏ. Ngay cả trên tất cả 10 máy, điều này vẫn nằm trong giới hạn bộ nhớ đã nêu và các thao tác nhỏ hơn nhiều so với cách triển khai \(O(2^m mn)\) ngây thơ. 

## Trường hợp thử nghiệm 

Bộ dây thử nghiệm sau đây sử dụng cùng một`solve`hoạt động như giải pháp được gửi. Mẫu chính thức được bao gồm chính xác như được công bố trong tuyên bố cuộc thi. citturn2view0```python
import sys
import io

def solve():
    input = sys.stdin.readline

    T = int(input())
    answers = []

   
        answer = m + 1

        for mask in range(1, total_masks):
            bit = mask & -mask
            index = bit.bit_length() - 1
            previous = mask ^ bit

            coverage[mask] = coverage[previous] | datasets[index]

            if coverage[mask] == full:
                answer = min(answer, mask.bit_count())

        answers.append(str(answer if answer <= m else -1))

    sys.stdout.write("\n".join(answers))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Official sample
sample = """\
5
3 3
100
011
111
5 6
10000
01001
01110
00111
10110
00101
6 7
000010
011000
100100
001000
000010
010000
110001
7 6
1001001
1001000
0001101
0010110
0110011
0100001
2 1
01
"""

assert run(sample) == """\
1
2
4
3
-1
""".strip(), "official sample"

# Minimum-size solvable case
assert run("""\
1
1 1
1
""") == "1", "single feature covered by one dataset"

# Minimum-size impossible case
assert run("""\
1
1 1
0
""") == "-1", "single feature is never covered"

# All datasets are identical, but one already covers everything
assert run("""\
1
4 3
1111
1111
1111
""") == "1", "duplicate full-coverage datasets"

# Coverage requires combining datasets
assert run("""\
1
3 2
100
011
""") == "2", "two datasets are necessary"

# Maximum m, with every dataset identical
max_case = "1\n1 15\n" + "1\n" * 15
assert run(max_case) == "1", "maximum number of datasets"

# Maximum n, one dataset covers every feature
max_n_case = "1\n500 1\n" + "1" * 500 + "\n"
assert run(max_n_case) == "1", "maximum number of features"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---:|---| 
|`1 / 1 1 / 1`|`1`| Trường hợp nhỏ nhất có thể giải được | 
|`1 / 1 1 / 0`|`-1`| Trường hợp nhỏ nhất không thể | 
| Ba giống hệt nhau`1111`bộ dữ liệu |`1`| Bộ dữ liệu trùng lặp và lựa chọn tối thiểu | 
|`100`Và`011`|`2`| Bảo hiểm yêu cầu kết hợp bộ dữ liệu | 
| Một tính năng và 15 bộ dữ liệu giống hệt nhau |`1`| Tối đa\(m\)ranh giới | 
| Một tập dữ liệu chứa 500 tập dữ liệu |`1`| Tối đa\(n\)ranh giới | 

## Vỏ cạnh 

Đối với trường hợp có thể giải quyết được một tính năng```text
1
1 1
1
```mặt nạ đầy đủ là`1`. Tập hợp con trống có phạm vi bảo hiểm`0`, trong khi tập hợp con`1`có bảo hiểm`1`, vì vậy câu trả lời trở thành`1`. Việc triển khai không bao giờ vô tình chấp nhận tập hợp con trống. 

Đối với trường hợp không thể có một tính năng```text
1
1 1
0
```mặt nạ đầy đủ lại xuất hiện`1`, nhưng tập con duy nhất không trống có phạm vi bao phủ`0`. Không có tập hợp con nào đạt đến mặt nạ đầy đủ, vì vậy trọng điểm vẫn còn`m + 1`và đầu ra là`-1`. 

Đối với các bộ dữ liệu có phạm vi bao phủ đầy đủ trùng lặp```text
1
4 3
1111
1111
1111
```mỗi tập dữ liệu đã bằng`full`. Tập hợp con tập dữ liệu đơn đầu tiên đạt đến phạm vi bao phủ đầy đủ và đưa ra câu trả lời về`1`. Các tập dữ liệu giống hệt bổ sung không thể cải thiện nó vì không có giải pháp hợp lệ nào có thể sử dụng ít hơn một tập dữ liệu. 

Đối với trường hợp cần kết hợp,```text
1
3 2
100
011
```tập dữ liệu đầu tiên bao gồm tính năng 1, tập dữ liệu thứ hai bao gồm các tính năng 2 và 3 và không tập hợp con riêng lẻ nào đạt được`111`. Tập hợp con chứa cả hai tập dữ liệu tạo ra`111`và có số lượng dân số`2`, đưa ra câu trả lời đúng. 

Để có số lượng tập dữ liệu tối đa,\(m=15\), thuật toán liệt kê chính xác 32768 tập hợp con. Đó là không gian tìm kiếm lớn nhất được phép đầu vào. Biểu diễn tập hợp con sử dụng 15 bit, do đó không cần xử lý đặc biệt khi trích xuất bit được đặt thấp nhất hoặc đếm các tập dữ liệu đã chọn. 

Để có được số lượng tính năng tối đa,\(n=500\),`full`chứa 500 bit một và mọi tập dữ liệu trở thành số nguyên Python 500 bit. Các số nguyên có độ chính xác tùy ý của Python xử lý việc này một cách trực tiếp, do đó không cần quản lý ranh giới tràn có chiều rộng cố định.
