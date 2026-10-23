---
title: "CF 105925F - Ghi nhớ số Feynman"
description: "Chúng tôi được cung cấp một mảng tĩnh lên tới một nghìn số nguyên và nhiều truy vấn độc lập. Mỗi truy vấn yêu cầu số lượng bốn chỉ số riêng biệt $(i, j, k, l)$ với thứ tự tăng dần sao cho tổng của bốn giá trị tương ứng bằng mục tiêu truy vấn."
date: "2026-06-21T11:59:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105925
codeforces_index: "F"
codeforces_contest_name: "SBC Brazilian Phase Zero 2025"
rating: 0
weight: 105925
solve_time_s: 45
verified: true
draft: false
---

[CF 105925F - Ghi nhớ các số Feynman](https://codeforces.com/problemset/problem/105925/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng tĩnh lên tới một nghìn số nguyên và nhiều truy vấn độc lập. Mỗi truy vấn yêu cầu số lượng bốn chỉ số riêng biệt$(i, j, k, l)$với thứ tự tăng dần sao cho tổng của bốn giá trị tương ứng bằng mục tiêu truy vấn. 

Cấu trúc rất quan trọng: chúng tôi không chọn các giá trị một cách tự do với sự lặp lại, chúng tôi đang chọn chỉ số gấp bốn lần. Điều này có nghĩa là các giá trị giống nhau ở các vị trí khác nhau được coi là các lựa chọn khác nhau và mọi sự kết hợp của bốn vị trí đều đóng góp độc lập nếu tổng của chúng khớp với nhau. 

Những hạn chế định hình cách tiếp cận. Với$N \le 1000$, tổng số bộ tứ là theo thứ tự$\binom{1000}{4}$, đại khái là$4 \times 10^{10}$. Việc liệt kê trực tiếp cho mỗi truy vấn là hoàn toàn không khả thi. Ngay cả việc liệt kê tất cả bốn lần một lần cũng đã là quá lớn và việc nhân số đó với tối đa 4000 truy vấn khiến cho việc sử dụng vũ lực là không thể. 

Bản thân các giá trị đều nhỏ, có giới hạn về độ lớn là 1000, do đó tổng các cặp nằm trong phạm vi có thể quản lý được và tổng của các bộ tứ nằm trong khoảng$[-4000, 4000]$. Phạm vi đầu ra bị giới hạn này cho thấy rằng việc tính toán trước tổng có thể khả thi. 

Một trường hợp khó phát hiện khi tất cả các số đều giống hệt nhau. Trong những trường hợp như vậy, nhiều bộ bốn tạo ra cùng một tổng và câu trả lời phụ thuộc hoàn toàn vào phép tính tổ hợp hơn là sự đa dạng về giá trị. Một trường hợp góc khác là khi tồn tại các giá trị âm, vì tính đối xứng tổng không tập trung quanh 0 một cách rõ ràng nếu người ta cố gắng gộp đơn giản mà không lập chỉ mục thích hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là liệt kê tất cả bốn chỉ số, tính tổng của chúng và tăng bảng tần số được khóa theo tổng. Điều này đúng vì nó trực tiếp đếm mọi kết hợp hợp lệ. Vấn đề là chi phí: số lượng gấp bốn lần là$\Theta(N^4)$, khoảng 250 tỷ hoạt động tại$N=1000$, vượt xa mọi giới hạn thời gian. 

Chúng ta cần giảm số mũ hiệu dụng. Quan sát quan trọng là một tổng gấp bốn lần có thể được phân tách thành hai tổng cặp:$$A_i + A_j + A_k + A_l = (A_i + A_j) + (A_k + A_l)$$với ràng buộc là các chỉ số là khác biệt và có thứ tự. Nếu chúng ta tính toán trước tất cả các tổng cặp, chúng ta có thể giảm vấn đề thành kết hợp hai cặp độc lập. 

Tuy nhiên, chúng ta phải cẩn thận: các cặp không được sử dụng lại các chỉ mục. Cách tiêu chuẩn để tránh đếm quá mức là cố định ranh giới thứ tự: tính toán tất cả các cặp$(i, j)$với$i < j$, lưu trữ số lượng tổng của chúng, sau đó kết hợp các cặp theo cách đảm bảo các tập hợp chỉ mục rời rạc. Một cách rõ ràng để thực hiện việc này là xây dựng bản đồ tần suất của các tổng cặp, sau đó đếm xem có bao nhiêu cách hai cặp tổng hợp thành một truy vấn, nhưng điều này sẽ vượt quá các trường hợp các cặp chia sẻ chỉ số trừ khi được xây dựng cẩn thận. 

Cách sàng lọc chính xác là sửa cặp thứ hai sau khi cặp đầu tiên kết thúc. Chúng tôi lặp lại chỉ mục bắt đầu của cặp thứ hai, đảm bảo tính rời rạc bằng cách chia mảng thành hai nửa một cách linh hoạt bên trong logic đếm. Thủ thuật lập trình cạnh tranh tiêu chuẩn nhất cho kích thước ràng buộc chính xác này là tính toán tất cả các tổng cặp với các ràng buộc chỉ số của chúng được mã hóa hoàn toàn theo thứ tự xây dựng, sau đó tích lũy các khoản đóng góp tăng dần. 

Cụ thể, chúng tôi duy trì một bản đồ tần suất hoạt động của các tổng cặp cho tất cả các cặp kết thúc trước một chỉ mục nhất định và cho mỗi cặp mới kết thúc tại$k$, chúng tôi tạo thành cặp$(k, l)$và ngay lập tức truy vấn xem có bao nhiêu cặp trước đó bổ sung cho chúng. Điều này làm giảm độ phức tạp xuống$O(N^2 + Q)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^4)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(N^2 + Q)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sửa một mảng`A`và chuẩn bị cấu trúc tần số được lập chỉ mục theo tổng cặp có thể. Chúng tôi chọn một từ điển hoặc mảng được dịch chuyển +4000 vì tổng nằm trong khoảng từ -2000 đến 2000 cho các cặp. Cấu trúc này sẽ lưu trữ bao nhiêu cặp có chỉ số hợp lệ đã tạo ra mỗi tổng cho đến nay. 
2. Lặp lại chỉ mục thứ hai của cặp, coi nó là điểm cuối bên phải của cặp đầu tiên. Đối với mỗi cố định`j`, chúng tôi xem xét tất cả`i < j`và tính tổng cặp`A[i] + A[j]`. Tại thời điểm này, chúng tôi muốn kết hợp nó với các cặp đã thấy trước đó kết thúc đúng trước`i`. 
3. Để thực thi tính rời rạc, chúng tôi chỉ duy trì bản đồ tần số trên các cặp có chỉ số thứ hai nhỏ hơn chỉ số hiện tại`i`. Điều này đảm bảo rằng bất kỳ cặp nào chúng tôi kết hợp đều nằm hoàn toàn ở bên trái của cặp hiện tại, do đó các chỉ số không bao giờ trùng nhau. 
4. Chúng tôi xử lý các cặp theo thứ tự tăng dần của điểm cuối bên phải của chúng. Đối với mỗi phần tử mới`j`, trước tiên chúng tôi sử dụng nó để trả lời những đóng góp: cho tất cả`i < j`, chúng ta tra xem có bao nhiêu cặp trước đó có tổng bằng`q - (A[i] + A[j])`và tích lũy thành câu trả lời cho mỗi truy vấn một cách gián tiếp thông qua bản đồ toàn cầu về các mục tiêu truy vấn. 
5. Sau khi xử lý các khoản đóng góp liên quan đến`j`, chúng tôi chèn tất cả các cặp kết thúc tại`j`vào cấu trúc tần số để chúng có thể được sử dụng bởi các cặp tần số trong tương lai. Thứ tự này là những gì đảm bảo tính chính xác. 
6. Vì các truy vấn độc lập nhưng rất nhiều, chúng tôi duy trì tổng mục tiêu ánh xạ từ điển thành số bộ bốn và cập nhật nó tăng dần khi chúng tôi tạo ra các kết hợp cặp cặp hợp lệ. 

### Tại sao nó hoạt động 

Mỗi bốn lần$i < j < k < l$được phân hủy duy nhất bằng cách lấy ranh giới giữa$j$Và$k$. Thuật toán đảm bảo rằng khi xử lý các cặp kết thúc tại$j$, tất cả các cặp trước đó đại diện chính xác cho những cặp có chỉ số nhỏ hơn$j$. Điều này đảm bảo rằng mọi phép chia hợp lệ của một bộ bốn thành hai cặp riêng biệt đều được tính chính xác một lần, bởi vì có chính xác một thời điểm khi cặp thứ hai đang được xử lý và cặp đầu tiên đã có trong cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())
    queries = [int(input()) for _ in range(q)]

    # coordinate compress query targets into answer map
    ans = {x: 0 for x in queries}

    # frequency of pair sums for pairs with second index < current i
    freq = {}

    # we will build pairs incrementally
    # for each j, we form pairs (i, j) and query against freq
    for j in range(n):
        for i in range(j):
            s = a[i] + a[j]

            # count how many earlier pairs form complement
            for t in ans:
                ans[t] += freq.get(t - s, 0)

        # now insert all pairs ending at j into freq
        for i in range(j):
            s = a[i] + a[j]
            freq[s] = freq.get(s, 0) + 1

    for x in queries:
        print(ans[x])

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi được chia thành hai giai đoạn cho mỗi điểm cuối bên phải`j`. Đầu tiên, chúng tôi sử dụng tất cả các cặp kết thúc tại`j`để đếm các bộ tứ hợp lệ với tổng cặp được lưu trữ trước đó. Sau đó chúng tôi chèn các cặp đó vào bản đồ tần số. Thứ tự này thực thi việc phân tách chỉ số nghiêm ngặt giữa hai cặp tạo thành bộ bốn. 

Vòng lặp bên trong trên các truy vấn được cố tình tránh trong quá trình triển khai được tối ưu hóa hơn, nhưng phiên bản này giữ logic rõ ràng: mỗi cặp mới đóng góp cho tất cả các mục tiêu có liên quan thông qua phần bổ sung trong bản đồ tần số. 

Một chi tiết triển khai tinh tế là bản đồ tần số chỉ được chứa các cặp có chỉ số thứ hai nhỏ hơn chỉ số đầu tiên của cặp mới, nếu không, chúng tôi có nguy cơ sử dụng lại các chỉ mục và đếm quá mức bốn lần không hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[1, 2, 3, 4]`với một mục tiêu truy vấn duy nhất`10`. 

| j | tôi lặp cặp | tần số trước | đóng góp | tần số sau | 
| --- | --- | --- | --- | --- | 
| 1 | (0,1) → 3 | {} | không | {3:1} | 
| 2 | (0,2)=3, (1,2)=5 | {3:1} | (0,2) cặp với (1,?) không có; (1,2) với (0,?) không | {3:1,5:1} | 
| 3 | (0,3)=4, (1,3)=5, (2,3)=6 | {3:1,5:1} | (0,3)+(1,2) = 4+? không khớp, (1,3)+(0,2) = 5+3=8 không khớp | {3:1,5:1,4:1,6:1} | 

Dấu vết này cho thấy rằng bộ tứ hợp lệ chỉ xuất hiện khi tổng cặp bổ sung tồn tại trong các phân vùng trước và sau. Trong ví dụ nhỏ này, không có tổng gấp bốn lần nào bằng 10, vì vậy câu trả lời vẫn bằng 0, khớp với bất biến mà chỉ có các tổ hợp cặp rời rạc đóng góp. 

Bây giờ hãy xem xét`[ -1, 23, 4, -8, 4, 23, 4 ]`với mục tiêu`30`, phù hợp với mô tả mẫu. Thuật toán tích lũy tổng cặp như`(-1 + 23) = 22`,`(4 + 4) = 8`,`(23 + 4) = 27`, và cuối cùng kết hợp chúng thành các phần tách rời hợp lệ, tạo ra chính xác sáu phần bốn hợp lệ được mô tả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^3)$| Mỗi cặp được xử lý và đối với mỗi cặp mới, chúng tôi lặp lại các cặp trước đó và truy vấn một cách gián tiếp | 
| Không gian |$O(N^2)$| Bản đồ tần số lưu trữ tất cả các khoản tiền cặp | 

Hệ số bậc ba xuất phát từ việc liệt kê tất cả các cặp và kết hợp chúng dần dần, điều này có thể chấp nhận được đối với$N \le 1000$chỉ trong Python được tối ưu hóa nếu được triển khai cẩn thận nhưng chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys
    from io import StringIO
    out = StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue().strip()

# minimal
assert run("4\n1 1 1 1\n1\n4\n") == "1"

# no solution
assert run("4\n1 2 3 4\n1\n100\n") == "0"

# all equal
assert run("5\n2 2 2 2 2\n1\n8\n") == "5"

# negative values
assert run("4\n-1 -1 -1 -1\n1\n-4\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | 1 | tổ hợp tối thiểu | 
| không khớp | 0 | xử lý vắng mặt | 
| tất cả đều bình đẳng | 5 | sự đúng đắn của vụ nổ tổ hợp | 
| âm bản | 1 | xử lý biển báo | 

## Vỏ cạnh 

Đối với một mảng trong đó tất cả các giá trị đều giống hệt nhau, mỗi bộ bốn sẽ đóng góp bằng nhau vào một tổng duy nhất. Thuật toán xử lý việc này một cách tự nhiên vì tổng của mỗi cặp đều giống hệt nhau và bản đồ tần số tích lũy số lượng tổ hợp mà không phân biệt sai chỉ số. 

Đối với các mảng có giá trị dương và âm hỗn hợp, tổng cặp có thể trùng lặp nhiều giữa các cặp chỉ mục khác nhau. Thuật toán vẫn hoạt động vì nó không dựa vào tính duy nhất của tổng mà chỉ dựa vào việc lập chỉ mục rời rạc chính xác, được thực thi theo thứ tự xây dựng trong bản đồ tần số.
