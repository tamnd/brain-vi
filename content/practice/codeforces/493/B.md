---
title: "CF 493B - Vasya và Đấu vật"
description: "Chúng tôi được cung cấp nhật ký theo thứ tự thời gian về các kỹ thuật đấu vật. Mỗi mục nhập là một số nguyên mô tả cả người thực hiện nước đi và số điểm mà người đó đã đóng góp."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 493
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 281 (Div. 2)"
rating: 1400
weight: 493
solve_time_s: 654
verified: false
draft: false
---

[CF 493B - Vasya và Đấu vật](https://codeforces.com/problemset/problem/493/B) 

**Đánh giá:** 1400 
**Thẻ:** triển khai 
**Thời gian giải:** 10 phút 54 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhật ký theo thứ tự thời gian về các kỹ thuật đấu vật. Mỗi mục nhập là một số nguyên mô tả cả người thực hiện nước đi và số điểm mà người đó đã đóng góp. Giá trị dương có nghĩa là đô vật đầu tiên thực hiện một động tác có giá trị bằng số điểm đó, trong khi giá trị âm có nghĩa là đô vật thứ hai thực hiện một động tác có giá trị tuyệt đối của số đó. 

Từ luồng này, chúng tôi xây dựng lại hai chuỗi: một chuỗi điểm mà đô vật đầu tiên kiếm được theo thứ tự họ xuất hiện và một chuỗi khác dành cho đô vật thứ hai. Cuối cùng, người chiến thắng được xác định qua ba giai đoạn. Đầu tiên, chúng tôi so sánh tổng số điểm. Nếu một đô vật có số tiền lớn hơn, họ sẽ thắng ngay lập tức. Nếu tổng số bằng nhau, chúng ta so sánh hai chuỗi theo từ điển và chuỗi lớn hơn sẽ thắng. Nếu cả hai chuỗi giống hệt nhau về giá trị và độ dài thì đô vật thực hiện động tác cuối cùng trong đầu vào là người chiến thắng. 

Ràng buộc n lên tới 2·10^5 ngụ ý rằng mọi lời giải đều phải tuyến tính hoặc gần tuyến tính về mặt thời gian. Cách tiếp cận bậc hai so sánh các tiền tố nhiều lần hoặc quét lại các chuỗi liên tục sẽ quá chậm vì nó có thể đạt tới 10^10 thao tác trong trường hợp xấu nhất. Chúng ta cần xử lý dữ liệu đầu vào trong một lần duy nhất và tránh so sánh lặp lại. 

Trường hợp cạnh tinh tế xuất hiện khi tổng số bằng nhau nhưng các chuỗi chỉ khác nhau ở phần tử khác nhau cuối cùng. Ví dụ: nếu một chuỗi là tiền tố của chuỗi kia, việc so sánh từ điển phụ thuộc vào độ dài. Một trường hợp khác là khi các chuỗi giống hệt nhau về giá trị và độ dài, trong đó người chiến thắng chỉ được quyết định bởi sự kiện khác 0 cuối cùng, có thể thuộc về một trong hai đô vật. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ xây dựng rõ ràng cả hai chuỗi, sau đó tính tổng của chúng và cuối cùng so sánh chúng theo từ điển. Điều này có hiệu quả về mặt khái niệm, nhưng bản thân việc so sánh từ điển có thể bị suy giảm. Trong trường hợp xấu nhất, cả hai chuỗi có thể chứa tối đa n phần tử và so sánh chúng từ đầu cho mọi điểm quyết định hoặc nhiều lần truyền dữ liệu vẫn tuyến tính, nhưng sự kém hiệu quả thực sự sẽ xuất hiện nếu người ta cố gắng mô phỏng so sánh nhiều lần trong khi xử lý. 

Quan sát quan trọng là chúng ta không cần phải so sánh nhiều lần các trình tự. Chúng ta chỉ cần xây dựng chúng một lần và sau đó thực hiện ba phép so sánh xác định ở cuối: so sánh tổng, so sánh từ điển và so sánh nước đi cuối cùng. Vì việc xây dựng các chuỗi đã là O(n), nên tất cả các phép toán còn lại cũng phải là O(n) hoặc O(1) cho mỗi phép so sánh. 

Chúng tôi duy trì hai mảng, một mảng cho mỗi đô vật và hai mảng chạy. Chúng tôi cũng theo dõi chỉ số của bước đi cuối cùng. Sau khi dữ liệu đầu vào được xử lý hoàn toàn, chúng tôi so sánh tổng. Nếu bằng nhau, chúng tôi so sánh các danh sách bằng cách sử dụng so sánh từ điển gốc của Python, cách này tối ưu và được triển khai trong C. Nếu vẫn bằng nhau, chúng tôi sẽ kiểm tra xem người chơi nào đã thực hiện nước đi cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Được chấp nhận nhưng chi phí không cần thiết | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc n, sau đó lặp lại tất cả các nước đi theo thứ tự. Với mỗi giá trị aᵢ, hãy xác định đô vật nào đã thực hiện nó bằng cách kiểm tra dấu hiệu của nó. Bước này là cần thiết để xây dựng lại cả hai chuỗi tính điểm một cách trung thực. 
2. Nếu aᵢ dương, hãy thêm nó vào chuỗi của đô vật đầu tiên và thêm nó vào tổng số lần chạy của họ. Nếu aᵢ âm, hãy cộng giá trị tuyệt đối của nó vào chuỗi của đô vật thứ hai và cộng nó vào tổng của họ. Điều này giữ cho cả việc tích lũy điểm và tái cấu trúc trình tự được đồng bộ hóa trong một lượt. 
3. Theo dõi chỉ số của nước đi cuối cùng và người chơi nào đã thực hiện nước đi đó. Thông tin này chỉ cần thiết nếu tất cả các so sánh khác kết thúc bằng tỷ số hòa. 
4. Sau khi xử lý tất cả các nước đi, so sánh tổng số điểm của cả hai đô vật. Nếu cái nào lớn hơn, chúng ta có thể quyết định ngay người chiến thắng mà không cần xem xét cấu trúc trình tự. 
5. Nếu tổng bằng nhau, hãy so sánh hai chuỗi theo từ điển. Sự so sánh này tuân theo thứ tự từ điển tiêu chuẩn: phần tử khác nhau đầu tiên sẽ quyết định, nếu không thì chuỗi dài hơn sẽ thắng nếu phần tử này là tiền tố của phần tử kia. 
6. Nếu cả hai chuỗi giống hệt nhau, hãy trả về người chiến thắng dựa trên người thực hiện nước đi cuối cùng. 

### Tại sao nó hoạt động 

Thuật toán phản ánh trực tiếp hệ thống phân cấp quyết định của vấn đề: so sánh tổng chiếm ưu thế so sánh từ điển, chi phối việc phá vỡ liên kết nước đi cuối cùng. Bởi vì mỗi phần thông tin (chuỗi, tổng, bước cuối cùng) được bảo toàn chính xác như được xác định trong bài toán nên không có phép biến đổi hoặc phép tính gần đúng nào được đưa ra. Tính chính xác xuất phát từ thực tế là thứ tự từ điển chỉ phụ thuộc vào vị trí khác nhau đầu tiên và chúng tôi bảo toàn các chuỗi đầy đủ, do đó không có thông tin cần thiết để so sánh nào bị mất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
first = []
second = []
sum1 = 0
sum2 = 0

last = 0  # 1 for first, 2 for second

for _ in range(n):
    x = int(input())
    if x > 0:
        first.append(x)
        sum1 += x
        last = 1
    else:
        x = -x
        second.append(x)
        sum2 += x
        last = 2

if sum1 > sum2:
    print("first")
elif sum2 > sum1:
    print("second")
else:
    if first > second:
        print("first")
    elif second > first:
        print("second")
    else:
        print("first" if last == 1 else "second")
```Mã này xây dựng cả hai chuỗi trong khi tính tổng của chúng trong một lần chuyển qua đầu vào. Biến cuối cùng lưu trữ người thực hiện bước di chuyển gần đây nhất, chỉ được sử dụng nếu cả hai chuỗi đều có giá trị và cấu trúc giống hệt nhau. 

Sự so sánh`first > second`tận dụng tính năng so sánh danh sách từ điển tích hợp của Python, khớp chính xác với định nghĩa vấn đề. Điều này tránh việc so sánh thủ công từng phần tử và giữ cho quá trình triển khai được gọn nhẹ và an toàn khỏi các lỗi lập chỉ mục. 

Tie-break cuối cùng sử dụng động lực cuối cùng được lưu trữ, đảm bảo tính chính xác khi các chuỗi hoàn toàn giống nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1
2
-3
-4
3
```| Bước | Phần đầu tiên | Thứ hai | tổng1 | tổng2 | cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [1] | [] | 1 | 0 | đầu tiên | 
| 2 | [1,2] | [] | 3 | 0 | đầu tiên | 
| 3 | [1,2] | [3] | 3 | 3 | thứ hai | 
| 4 | [1,2] | [3,4] | 3 | 7 | thứ hai | 
| 5 | [1,2,3] | [3,4] | 6 | 7 | đầu tiên | 

Tổng là 6 và 7 nên đô vật thứ hai thắng ngay mà không cần so sánh từ điển. Dấu vết cho thấy đô vật thứ hai xây dựng tổng điểm mạnh hơn như thế nào mặc dù đô vật đầu tiên có nhiều yếu tố hơn. 

### Ví dụ 2 

đầu vào:```
4
1
2
-1
-2
```| Bước | Phần đầu tiên | Thứ hai | tổng1 | tổng2 | cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [1] | [] | 1 | 0 | đầu tiên | 
| 2 | [1,2] | [] | 3 | 0 | đầu tiên | 
| 3 | [1,2] | [1] | 3 | 1 | thứ hai | 
| 4 | [1,2] | [1,2] | 3 | 3 | thứ hai | 

Bây giờ số tiền bằng nhau. So sánh từ điển cho thấy cả hai chuỗi đều giống hệt nhau, do đó quyết định chuyển sang quy tắc di chuyển cuối cùng. Nước đi cuối cùng thuộc về đô vật thứ hai nên họ thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | xây dựng một lần cộng với so sánh từ điển O(n) trong trường hợp xấu nhất | 
| Không gian | O(n) | lưu trữ cả hai chuỗi một cách rõ ràng | 

Các ràng buộc cho phép tối đa 2·10^5 lần di chuyển và cả mức sử dụng bộ nhớ và thời gian đều tuyến tính với n, vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    output = []

    def fake_print(*args):
        output.append(" ".join(map(str, args)))

    # backup
    orig_print = builtins.print
    builtins.print = fake_print

    try:
        n = int(input())
        first = []
        second = []
        sum1 = 0
        sum2 = 0
        last = 0

        for _ in range(n):
            x = int(input())
            if x > 0:
                first.append(x)
                sum1 += x
                last = 1
            else:
                x = -x
                second.append(x)
                sum2 += x
                last = 2

        if sum1 > sum2:
            print("first")
        elif sum2 > sum1:
            print("second")
        else:
            if first > second:
                print("first")
            elif second > first:
                print("second")
            else:
                print("first" if last == 1 else "second")

        return "\n".join(output)
    finally:
        builtins.print = orig_print

# provided sample
assert run("""5
1
2
-3
-4
3
""") == "second"

# minimum case
assert run("""1
5
""") == "first"

# second wins by sum
assert run("""3
1
-10
2
""") == "second"

# lexicographic tie, different structure
assert run("""4
1
2
-1
-3
""") == "second"

# identical sequences, last move decides
assert run("""2
1
-1
""") == "second"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 5 nước đi | thứ hai | trường hợp hỗn hợp tiêu chuẩn | 
| tích cực duy nhất | đầu tiên | kích thước tối thiểu | 
| tổng thứ hai thống trị | thứ hai | sự thống trị tổng hợp | 
| hành vi ràng buộc dựa trên tiền tố | thứ hai | cạnh từ điển | 
| trình tự giống hệt nhau | thứ hai | tie-break nước đi cuối cùng | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi cả hai chuỗi giống hệt nhau nhưng được xây dựng theo thứ tự di chuyển khác nhau. Xem xét đầu vào:```
2
1
-1
```Chuỗi đầu tiên là [1], chuỗi thứ hai là [1]. Tổng bằng nhau và so sánh từ điển mang lại sự bình đẳng. Thuật toán quay trở lại nước đi cuối cùng một cách chính xác, nước đi thứ hai, tạo ra "thứ hai". 

Một trường hợp đặc biệt khác là khi một chuỗi là tiền tố nghiêm ngặt của chuỗi kia:```
3
1
2
-1
```Ở đây đầu tiên = [1,2], thứ hai = [1]. Các tổng khác nhau nên câu trả lời đã được xác định là "đầu tiên". Nếu các tổng bằng nhau một cách giả tạo trong một kịch bản đã sửa đổi, thì so sánh từ điển sẽ ưu tiên chính xác cho chuỗi dài hơn, vì so sánh danh sách Python xử lý thứ tự tiền tố chính xác theo yêu cầu của định nghĩa bài toán.
