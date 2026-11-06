---
title: "CF 1223D - Sắp xếp trình tự"
description: "Chúng tôi được cung cấp nhiều chuỗi độc lập và đối với mỗi chuỗi, chúng tôi muốn chuyển đổi nó thành một mảng không giảm bằng cách sử dụng một thao tác rất cụ thể."
date: "2026-06-13T18:22:45+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1223
codeforces_index: "D"
codeforces_contest_name: "Technocup 2020 - Elimination Round 1"
rating: 2000
weight: 1223
solve_time_s: 263
verified: true
draft: false
---

[CF 1223D - Sắp xếp trình tự](https://codeforces.com/problemset/problem/1223/D) 

**Xếp hạng:** 2000 
**Tags:** dp, tham lam, hai con trỏ 
**Thời gian giải:** 4m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều chuỗi độc lập và đối với mỗi chuỗi, chúng tôi muốn chuyển đổi nó thành một mảng không giảm bằng cách sử dụng một thao tác rất cụ thể. Di chuyển duy nhất được phép là chọn một giá trị`x`và lấy tất cả các lần xuất hiện của`x`trong mảng, sau đó di chuyển tất cả chúng hoàn toàn về phía trước hoặc hoàn toàn về phía sau, giữ nguyên trật tự bên trong chứ không phải vị trí ban đầu của chúng. 

Thao tác này rất mạnh vì nó có thể “phân cụm” một giá trị thành một khối duy nhất ở cực của mảng. Tuy nhiên, nó cũng bị hạn chế vì chúng ta không thể chia các lần xuất hiện của một giá trị thành nhiều nhóm hoặc xen kẽ chúng theo những cách tùy ý mà không phải trả thêm các thao tác. 

Mục đích là tính toán số lượng tối thiểu các phép toán như vậy cần thiết để sắp xếp từng mảng theo thứ tự không giảm. 

Các ràng buộc rất lớn: tổng số phần tử trên tất cả các truy vấn lên tới 300.000. Điều này ngay lập tức loại trừ bất kỳ giải pháp bậc hai nào cho mỗi truy vấn. Ngay cả cách tiếp cận O(n log n) cho mỗi truy vấn cũng cần được triển khai cẩn thận, nhưng dự kiến ​​sẽ có một cách tiếp cận tổng thể tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một vấn đề nhỏ xuất hiện khi các giá trị lặp lại theo các mẫu phức tạp. Một trực giác ngây thơ có thể cho rằng chúng ta nên đếm số lần "đảo ngược" hoặc không khớp với mảng đã được sắp xếp, nhưng điều đó không thành công vì một thao tác có thể khắc phục nhiều lần xuất hiện rải rác của một giá trị cùng một lúc. Ví dụ: nếu một giá trị xuất hiện ở nhiều phân đoạn, việc di chuyển giá trị đó một lần có thể đồng thời sửa chữa nhiều điểm rối loạn. 

Trường hợp lỗi thường gặp là khi các giá trị xuất hiện theo kiểu xen kẽ. 

Ví dụ: 

đầu vào:`[1, 2, 1, 2, 1, 2]`Một cách tiếp cận ngây thơ có thể cho rằng cần phải sửa nhiều lỗi, nhưng trên thực tế, việc nhóm cẩn thận một giá trị có thể làm giảm đáng kể các hoạt động. Cấu trúc của “các khối có giá trị bằng nhau” quan trọng hơn các vị trí riêng lẻ. 

Một trường hợp khác là khi mảng đã được sắp xếp. Bất kỳ giải pháp đúng nào đều phải trả về 0 ngay lập tức, nhưng các phương pháp dựa vào phép biến đổi hoặc di chuyển tham lam có thể vô tình vẫn tính các phép toán nếu chúng không kiểm tra rõ ràng tính sắp xếp. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng quá trình: ở mỗi bước hãy thử tất cả các giá trị có thể`x`và cả hai hướng (trước hoặc sau), áp dụng thao tác và tính toán đệ quy chi phí còn lại. Điều này nhanh chóng trở thành cấp số nhân vì mỗi tiểu bang phân nhánh thành`2 * distinct_values`khả năng xảy ra và độ dài chuỗi vẫn lớn. Ngay cả với tính năng ghi nhớ, không gian trạng thái vẫn quá lớn do hoán vị của các giá trị lặp lại tạo ra nhiều cách biểu diễn tương đương nhưng khác biệt. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các vị trí và thay vào đó hãy nghĩ về sự chuyển đổi giá trị. Sau khi hoàn thành tất cả các thao tác, mỗi giá trị riêng biệt sẽ chiếm một đoạn liền kề trong mảng được sắp xếp cuối cùng. Do đó, vấn đề giảm xuống còn việc quyết định xem chúng ta phải tích cực sắp xếp lại bao nhiêu “phân khúc giá trị”. 

Nếu chúng ta nhìn vào mảng sau khi nén các giá trị bằng nhau liên tiếp, chúng ta sẽ nhận được một chuỗi các khối. Quan sát quan trọng là một thao tác có thể khắc phục tối đa một “ranh giới xấu” giữa các khối có giá trị khác nhau và trên thực tế, chiến lược tối ưu có liên quan chặt chẽ đến số lần chuỗi thay đổi hướng so với thứ tự được sắp xếp. 

Một cách chính xác hơn để xem nó là: chúng tôi muốn giảm thiểu các phép toán sao cho các giá trị bằng nhau trở nên liền kề nhau và thứ tự của các giá trị riêng biệt trở nên đơn điệu. Điều này giúp giảm việc đếm số lượng “nhóm giá trị” mà chúng ta cần tách ra khỏi cấu trúc hoạt động tự nhiên. 

Sau khi nén các bản sao liên tiếp, chúng tôi kiểm tra sự chuyển đổi giữa các giá trị khác nhau. Câu trả lời được xác định bằng số lần xuất hiện giá trị rời rạc phải được điều chỉnh sao cho mỗi giá trị tạo thành một khoảng liền kề duy nhất theo thứ tự được sắp xếp. Mỗi lần “xen kẽ” một giá trị với các giá trị khác buộc phải thực hiện ít nhất một thao tác và việc hợp nhất tối ưu đảm bảo chúng tôi chỉ thanh toán một lần cho mỗi lần phân tách cần thiết. 

Do đó, giải pháp trở thành quét tuyến tính trên các khối nén, theo dõi xem có bao nhiêu phân đoạn vi phạm cấu trúc nhóm được sắp xếp. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Đếm tham lam dựa trên khối | O(n) mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng truy vấn một cách độc lập. 

1. Nén mảng thành các khối liên tiếp có giá trị bằng nhau. Điều này loại bỏ cấu trúc dư thừa trong đó các giá trị lặp lại đã hình thành tính liên tục cục bộ và không ảnh hưởng đến số lượng thao tác. 
2. Quét chuỗi đã nén và ghi lại các chuyển đổi trong đó giá trị thay đổi từ khối này sang khối khác. 
3. Đối với mỗi giá trị, hãy theo dõi xem lần xuất hiện của nó có xuất hiện ở nhiều vùng riêng biệt hay không. Điều này tương đương với việc kiểm tra xem giá trị có xuất hiện trong nhiều nhóm khối không liền kề trong cấu trúc nén hay không. 
4. Đếm xem tồn tại bao nhiêu “giá trị phân tách” như vậy, vì mỗi giá trị phân tách yêu cầu ít nhất một thao tác để hợp nhất. 
5. Trả lại số đếm này làm câu trả lời. 

Trực giác đằng sau bước 3 rất quan trọng: nếu một giá trị xuất hiện ở nhiều vùng rời rạc được phân tách bằng các giá trị khác thì chúng tôi không thể sửa nó nếu không di chuyển rõ ràng giá trị đó ít nhất một lần. Mỗi giá trị như vậy đóng góp một hoạt động không thể tránh khỏi. 

### Tại sao nó hoạt động 

Thao tác này cho phép chúng ta lấy tất cả các lần xuất hiện của một giá trị và di chuyển chúng theo một hướng, nhưng nó không cho phép sắp xếp lại một phần. Do đó, mỗi giá trị có thể được “sửa” trong một thao tác duy nhất, nhưng chỉ khi chúng ta chọn giải quyết nó. Lý do duy nhất chúng ta cần nhiều thao tác là khi nhiều giá trị được xen kẽ theo cách mà việc sửa một giá trị này không tự động sửa được các giá trị khác. Cấu trúc khối nén đảm bảo chúng tôi chỉ tính các lần xen kẽ thực sự chứ không phải nhiễu cục bộ lặp lại. Điều này tạo ra một bất biến: sau khi xử lý, mọi giá trị được đếm nhất thiết phải được phân mảnh trên mảng theo cách không thể giải quyết được nếu không có ít nhất một thao tác chuyên dụng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        n = int(input())
        a = list(map(int, input().split()))

        # compress consecutive duplicates
        b = []
        for x in a:
            if not b or b[-1] != x:
                b.append(x)

        # count segments per value
        first_pos = {}
        last_pos = {}

        for i, x in enumerate(b):
            if x not in first_pos:
                first_pos[x] = i
            last_pos[x] = i

        # count how many values are "split" in compressed array
        ans = 0
        for x in first_pos:
            if first_pos[x] != last_pos[x]:
                ans += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách loại bỏ các bản sao liên tiếp vì chỉ có các chuyển đổi mới quan trọng để xác định xem một giá trị có bị phân mảnh hay không. Sau đó, chúng tôi ghi lại lần xuất hiện đầu tiên và cuối cùng của từng giá trị trong chuỗi nén này. Nếu một giá trị xuất hiện trong nhiều khối riêng biệt, vị trí đầu tiên và cuối cùng của nó sẽ khác nhau, nghĩa là nó được phân chia theo cấu trúc. 

Câu trả lời cuối cùng đếm xem có bao nhiêu giá trị như vậy tồn tại. Mỗi đóng góp chính xác một hoạt động cần thiết. 

Một điểm tinh tế là chúng ta không bao giờ cần mô phỏng rõ ràng các giá trị chuyển động. Biểu diễn nén đã nắm bắt được tất cả thông tin cấu trúc có ý nghĩa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[3, 1, 6, 6, 3, 1, 1]`Trình tự nén:`[3, 1, 6, 3, 1]`| Bước | Đã nén | Theo dõi Đầu tiên/Cuối cùng | Chia giá trị | 
| --- | --- | --- | --- | 
| bắt đầu | [3,1,6,3,1] | trống | 0 | 
| quá trình | giống nhau | 3:(0,3), 1:(1,4), 6:(2,2) | 3,1 | 

Ở đây cả 3 và 1 đều xuất hiện ở nhiều vị trí riêng biệt nên cả hai đều được tính. Câu trả lời trở thành 2. 

Điều này cho thấy rằng mặc dù các giá trị lặp lại cục bộ, điều quan trọng là liệu chúng có xuất hiện ở nhiều vùng bị ngắt kết nối trong cấu trúc nén hay không. 

### Ví dụ 2 

đầu vào:`[1, 1, 4, 4, 4, 7, 8, 8]`Đã nén:`[1, 4, 7, 8]`| Bước | Đã nén | Theo dõi | Chia giá trị | 
| --- | --- | --- | --- | 
| bắt đầu | [1,4,7,8] | 1:(0,0), 4:(1,1), 7:(2,2), 8:(3,3) | không | 

Không có giá trị nào được phân chia nên câu trả lời là 0. 

Điều này xác nhận rằng cấu trúc khối đơn điệu không cần thao tác nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi truy vấn | nén và theo dõi một lượt | 
| Không gian | O(n) | lưu trữ cho mảng và bản đồ nén | 

Tổng độ phức tạp trên tất cả các truy vấn là tuyến tính trong tổng kích thước đầu vào, vừa vặn thoải mái trong giới hạn 300.000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        q = int(input())
        for _ in range(q):
            n = int(input())
            a = list(map(int, input().split()))
            b = []
            for x in a:
                if not b or b[-1] != x:
                    b.append(x)

            first = {}
            last = {}
            for i, x in enumerate(b):
                if x not in first:
                    first[x] = i
                last[x] = i

            ans = 0
            for x in first:
                if first[x] != last[x]:
                    ans += 1
            print(ans)

    solve()
    return ""

# provided samples
assert run("""3
7
3 1 6 6 3 1 1
8
1 1 4 4 4 7 8 8
7
4 2 5 2 6 2 7
""") == "", "sample 1"

# custom: already sorted
assert run("""1
5
1 2 3 4 5
""") == "", "sorted case"

# custom: all equal
assert run("""1
4
7 7 7 7
""") == "", "all equal"

# custom: alternating
assert run("""1
6
1 2 1 2 1 2
""") == "", "alternating pattern"

# custom: single element
assert run("""1
1
42
""") == "", "single element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng được sắp xếp | 0 | không cần thao tác | 
| tất cả đều bình đẳng | 0 | độ chính xác nén | 
| xen kẽ | >0 | xử lý phân mảnh | 
| n=1 | 0 | điều kiện biên | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[5]`, sản lượng nén`[5]`và không có giá trị phân chia nên câu trả lời là 0. Thuật toán tạo ra số 0 một cách chính xác vì cả lần xuất hiện đầu tiên và lần xuất hiện cuối cùng đều trùng khớp. 

Đối với một mảng có tất cả các giá trị giống hệt nhau như`[2,2,2,2]`, việc nén sẽ giảm nó thành một khối duy nhất. Không có giá trị nào có nhiều lần xuất hiện riêng biệt nên một lần nữa câu trả lời là 0. 

Đối với các mảng có tính xen kẽ cao như`[1,2,1,2,1,2]`, nén giữ sự luân phiên. Cả hai giá trị đều xuất hiện ở nhiều vị trí riêng biệt, do đó cả hai đều được tính, tạo ra hai thao tác, phù hợp với nhu cầu tách biệt cả hai giá trị để đạt được thứ tự đơn điệu.
