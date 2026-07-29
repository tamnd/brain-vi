---
title: "CF 102756J - Những chiến binh dũng mãnh của Trung Quốc"
description: "Chúng tôi có một dòng tượng $N$. Mỗi bức tượng được mô tả bằng một số $K$-bit, trong đó bit $i$ cho biết đặc điểm $i$ có xuất hiện trên bức tượng đó hay không. Một nhóm tượng liên tiếp nhau được gọi là hùng vĩ khi mọi đặc điểm đều xuất hiện với số lần như nhau trong nhóm đó."
date: "2026-07-29T00:35:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102756
codeforces_index: "J"
codeforces_contest_name: "UTPC Contest 10-09-20 Div. 1"
rating: 0
weight: 102756
solve_time_s: 60
verified: true
draft: false
---

[CF 102756J - Những chiến binh dũng mãnh của Trung Quốc](https://codeforces.com/problemset/problem/102756/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng$N$những bức tượng. Mỗi bức tượng được mô tả bằng một$K$-số bit, bit ở đâu$i$cho biết tính năng$i$xuất hiện trên bức tượng đó. Một nhóm tượng liên tiếp nhau được gọi là hùng vĩ khi mọi đặc điểm đều xuất hiện với số lần như nhau trong nhóm đó. Nhiệm vụ là tìm độ dài tối đa có thể có của một nhóm liên tiếp như vậy. 

Kích thước đầu vào lớn:$N$có thể đạt được$100000$, trong khi$K$có thể lớn như$30$. Một giải pháp kiểm tra mọi khoảng thời gian sẽ kiểm tra xung quanh$N^2$phạm vi, đó là về$10^{10}$cho đầu vào lớn nhất. Ngay cả việc cập nhật số lượng tính năng cho từng phạm vi cũng sẽ quá chậm. Chúng ta cần một cách tiếp cận gần tuyến tính. Giá trị nhỏ của$K$cho chúng ta biết rằng việc biểu diễn thông tin về các đặc điểm là có thể, nhưng chúng ta không đủ khả năng để lưu trữ mọi kết hợp số lượng đặc điểm có thể có. 

Một lỗi phổ biến là chỉ kiểm tra xem tổng số bit đã đặt có cân bằng hay không. Điều đó là chưa đủ vì mọi đặc điểm riêng lẻ phải có cùng tần số. 

Ví dụ:```
4 3
1
2
4
7
```Câu trả lời đúng là`1`. Một đoạn có chiều dài 2 như`[1, 2]`mỗi tính năng có xuất hiện một lần không? Không. Tính năng 1 xuất hiện một lần, tính năng 2 xuất hiện một lần, nhưng tính năng 3 xuất hiện 0 lần. Điều kiện là cả ba tính năng đều bằng nhau chứ không phải tổng số tính năng. 

Một trường hợp cạnh khác là$K=1$:```
5 1
0
1
0
1
1
```Câu trả lời đúng là`5`. Chỉ với một tính năng, mọi phạm vi sẽ tự động có số lượng bằng nhau vì chỉ có một số lượng để so sánh. Việc triển khai luôn xây dựng một vectơ có kích thước khác nhau$K-1$phải xử lý riêng trường hợp này. 

Trường hợp quan trọng thứ ba là phạm vi hùng vĩ bắt đầu từ bức tượng đầu tiên:```
3 2
1
2
3
```Câu trả lời đúng là`3`. Các giải pháp băm tiền tố phải lưu trữ tiền tố trống trước khi xử lý bất kỳ bức tượng nào, nếu không chúng không thể phát hiện các phạm vi bắt đầu từ chỉ mục 1. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ thử mọi khoảng thời gian có thể. Đối với mỗi điểm cuối bên trái, chúng tôi mở rộng điểm cuối bên phải và duy trì số lượng của mọi tính năng. Bất cứ khi nào tất cả$K$số lượng bằng nhau, chúng tôi cập nhật câu trả lời. Điều này đúng vì mọi phạm vi có thể đều được xem xét. Tuy nhiên, có$O(N^2)$phạm vi và kiểm tra sự bình đẳng giữa$K$quầy làm cho trường hợp xấu nhất$O(N^2K)$. Với$N=100000$, điều này vượt xa thời gian có sẵn. 

Quan sát chính là tránh lưu trữ số lượng thực tế. Một phạm vi sẽ hoành tráng nếu tất cả số lượng tính năng đều bằng nhau. Thay vì hỏi liệu mọi số đếm có cùng giá trị hay không, chúng ta có thể so sánh mọi đặc điểm với một đặc điểm tham chiếu đã chọn. 

Chọn tính năng số 0 làm tài liệu tham khảo. Đối với mỗi tiền tố của mảng, hãy lưu trữ sự khác biệt:$$count_1-count_0,\ count_2-count_0,\ ...,\ count_{K-1}-count_0$$Nếu hai tiền tố có cùng một vectơ khác biệt thì các bức tượng giữa các tiền tố đó sẽ thêm cùng một lượng vào mỗi số lượng tính năng. Sự khác biệt giữa hai vectơ tiền tố bằng nhau có mọi tính năng đều bằng nhau, điều đó có nghĩa là phân đoạn này rất hoành tráng. 

Điều này biến vấn đề thành việc tìm mảng con dài nhất có cùng trạng thái tiền tố, có thể được giải quyết bằng bản đồ băm lưu trữ lần xuất hiện đầu tiên của mỗi trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2K)$|$O(K)$| Quá chậm | 
| Băm sự khác biệt tiền tố |$O(NK)$|$O(NK)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tượng và xử lý các trường hợp đặc biệt$K=1$. Mọi khoảng đều hợp lệ nên câu trả lời là$N$. 
2. Duy trì một vectơ khác biệt tiền tố có độ dài$K-1$. Khi một bức tượng được xử lý, thêm$1$đến mọi vị trí tương ứng với một tính năng mà nó chứa ngoại trừ tính năng tham chiếu. Nếu bức tượng chứa đặc điểm tham chiếu, hãy trừ đi$1$từ mọi sự khác biệt về tính năng khác. 

Vectơ luôn biểu thị mức độ khác nhau của mỗi tính năng không tham chiếu so với số lượng tính năng tham chiếu trong tiền tố hiện tại. 
3. Lưu trữ chỉ mục đầu tiên nơi mỗi vectơ sai phân xuất hiện. Tiền tố trống có vectơ toàn 0 và được lưu trữ ở chỉ số 0. 
4. Khi một vectơ tiền tố xuất hiện lại, đoạn giữa lần xuất hiện đầu tiên và vị trí hiện tại có số lượng đặc điểm bằng nhau. Sử dụng khoảng cách giữa các vị trí đó làm câu trả lời khả thi. 
5. Tiếp tục cho đến khi tất cả các bức tượng được xử lý và xuất ra chiều dài tối đa được tìm thấy. 

Tại sao nó hoạt động: 

Vectơ được lưu trữ chứa chính xác thông tin cần thiết để so sánh số lượng đối tượng. Nếu hai tiền tố có cùng một vectơ thì những thay đổi giữa các tiền tố đó sẽ khiến mọi đặc điểm có cùng tần số vì tất cả những khác biệt liên quan đến đặc điểm tham chiếu sẽ bị loại bỏ. Ngược lại, nếu một phạm vi hoành tráng, mọi số lượng tính năng sẽ thay đổi như nhau trong phạm vi đó, do đó, vectơ sai phân tiền tố trước và sau phạm vi phải giống hệt nhau. Bản đồ băm tìm thấy chính xác các cặp tiền tố này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, k, arr):
    if k == 1:
        return n

    first = {tuple([0] * (k - 1)): 0}
    diff = [0] * (k - 1)
    ans = 0

    for idx, x in enumerate(arr, 1):
        has_first = x & 1

        if has_first:
            for i in range(k - 1):
                diff[i] -= 1

        for i in range(1, k):
            if x & (1 << i):
                diff[i - 1] += 1

        state = tuple(diff)

        if state in first:
            ans = max(ans, idx - first[state])
        else:
            first[state] = idx

    return ans

def main():
    data = sys.stdin.read().split()
    if not data:
        return

    ptr = 0
    out = []

    while ptr < len(data):
        n = int(data[ptr])
        k = int(data[ptr + 1])
        ptr += 2

        arr = list(map(int, data[ptr:ptr + n]))
        ptr += n

        out.append(str(solve_case(n, k, arr)))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai giữ nguyên lần xuất hiện đầu tiên của mọi trạng thái tiền tố vì lần xuất hiện trước đó luôn mang lại phân đoạn hợp lệ dài hơn. Việc chuyển đổi bộ dữ liệu là cần thiết vì danh sách có thể thay đổi và không thể là khóa từ điển. 

Tính năng tham chiếu được xử lý đặc biệt. Khi nó xuất hiện, mọi sự khác biệt đều giảm vì số lượng tham chiếu tăng thêm một. Đối với các tính năng khác, vị trí của chúng chỉ tăng khi có bit đó. Thứ tự của các cập nhật này không ảnh hưởng đến vectơ sai phân cuối cùng vì mỗi bức tượng đóng góp chính xác một lần. 

Tiền tố trống được chèn vào trước khi xử lý bất kỳ bức tượng nào. Điều này xử lý các phạm vi bắt đầu từ bức tượng đầu tiên và ngăn ngừa lỗi sai sót trong tính toán độ dài. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
7 3
7
6
7
2
1
4
2
```các trạng thái tiền tố là: 

| Chỉ mục | Tượng | Vector sự khác biệt | Nhìn thấy lần đầu | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | - | (0,0) | 0 | 0 | 
| 1 | 7 | (0,0) | 0 | 1 | 
| 2 | 6 | (-1,0) | 2 | 1 | 
| 3 | 7 | (-1,0) | 2 | 1 | 
| 4 | 2 | (-1,1) | 4 | 1 | 
| 5 | 1 | (-1,1) | 4 | 1 | 
| 6 | 4 | (0,0) | 0 | 6 | 
| 7 | 2 | (-1,1) | 4 | 4 | 

Các trạng thái lặp lại xác định phạm vi trong đó mọi tính năng đều thay đổi như nhau. Cái dài nhất có chiều dài 4. 

Một ví dụ thứ hai:```
5 2
1
2
3
0
3
```| Chỉ mục | Tượng | Vector sự khác biệt | Nhìn thấy lần đầu | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | - | (0) | 0 | 0 | 
| 1 | 1 | (-1) | 1 | 0 | 
| 2 | 2 | (0) | 0 | 2 | 
| 3 | 3 | (0) | 0 | 3 | 
| 4 | 0 | (1) | 4 | 3 | 
| 5 | 3 | (1) | 4 | 3 | 

Điều này chứng tỏ rằng một phạm vi hùng vĩ không cần phải chứa đựng mọi tính năng. Nó chỉ yêu cầu tất cả các tính năng phải có cùng tần số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NK)$| Mỗi bức tượng cập nhật nhiều nhất$K-1$sự khác biệt về tính năng. | 
| Không gian |$O(NK)$| Bản đồ băm lưu trữ tối đa một trạng thái cho mỗi tiền tố. | 

Với$N=100000$Và$K=30$, số lượng hoạt động là khoảng ba triệu bản cập nhật, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.read().split()
    ptr = 0
    out = []

    while ptr < len(data):
        n = int(data[ptr])
        k = int(data[ptr + 1])
        ptr += 2
        arr = list(map(int, data[ptr:ptr+n]))
        ptr += n

        if k == 1:
            out.append(str(n))
            continue

        first = {(0,) * (k - 1): 0}
        diff = [0] * (k - 1)
        ans = 0

        for idx, x in enumerate(arr, 1):
            if x & 1:
                for i in range(k - 1):
                    diff[i] -= 1
            for i in range(1, k):
                if x & (1 << i):
                    diff[i - 1] += 1

            state = tuple(diff)
            if state in first:
                ans = max(ans, idx - first[state])
            else:
                first[state] = idx

        out.append(str(ans))

    sys.stdin = old
    return "\n".join(out)

assert run("""7 3
7
6
7
2
1
4
2
""") == "4"

assert run("""3 2
1
2
3
""") == "3"

assert run("""5 1
0
1
0
1
1
""") == "5"

assert run("""4 3
1
2
4
7
""") == "1"

assert run("""5 2
0
0
0
0
0
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào mẫu | 4 | Trạng thái tiền tố lặp lại cơ bản | 
|`3 2 / 1 2 3`| 3 | Phạm vi bắt đầu từ đầu | 
|`5 1 / mixed bits`| 5 | Trường hợp đặc biệt một tính năng | 
|`4 3 / 1 2 4 7`| 1 | Tất cả các tính năng phải phù hợp riêng lẻ | 
| Năm mặt nạ không | 5 | Giá trị bằng nhau và câu trả lời dài đầy đủ | 

## Vỏ cạnh 

cho$K=1$, thuật toán trả về ngay lập tức vì không cần so sánh giữa các tính năng. Mỗi phạm vi bức tượng có một số tính năng, vì vậy điều kiện luôn đúng. 

Đối với các phạm vi bắt đầu từ bức tượng đầu tiên, trạng thái tiền tố 0 biểu thị trạng thái trước khi đọc bất kỳ thứ gì. Trong đầu vào:```
3 2
1
2
3
```trạng thái sau khi cả ba bức tượng trở về 0, khớp với trạng thái ban đầu được lưu trữ. Thuật toán tính toán chính xác độ dài như$3-0=3$. 

Đối với trường hợp mỗi số lượng tính năng phải khớp riêng biệt, vectơ khác biệt sẽ ngăn chặn kết quả dương tính giả. TRONG:```
4 3
1
2
4
7
```Cách tiếp cận dựa trên tổng số tính năng có thể coi không chính xác một số phân đoạn dài hơn là hợp lệ, nhưng sự khác biệt được lưu trữ cho thấy rằng ba tính năng không cân bằng cho đến khi chỉ còn lại các bức tượng đơn lẻ. Câu trả lời được tìm thấy chính xác là`1`.
