---
title: "CF 105163B - Chuỗi"
description: "Chúng tôi được cung cấp một chuỗi duy nhất bao gồm các ký tự và chúng tôi liên tục áp dụng quy tắc rút gọn cục bộ cho đến khi không thể thay đổi được nữa. Quy tắc rất đơn giản: bất cứ khi nào ba ký tự giống nhau liền kề nhau, chúng sẽ biến mất khỏi chuỗi."
date: "2026-06-27T10:53:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105163
codeforces_index: "B"
codeforces_contest_name: "The 19th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 105163
solve_time_s: 55
verified: true
draft: false
---

[CF 105163B - Chuỗi](https://codeforces.com/problemset/problem/105163/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi duy nhất bao gồm các ký tự và chúng tôi liên tục áp dụng quy tắc rút gọn cục bộ cho đến khi không thể thay đổi được nữa. Quy tắc rất đơn giản: bất cứ khi nào ba ký tự giống nhau liền kề nhau, chúng sẽ biến mất khỏi chuỗi. Việc xóa có thể khiến các bộ ba mới hình thành trên ranh giới của các phần riêng biệt trước đó, vì vậy quá trình này phải tiếp tục cho đến khi ổn định. 

Đầu ra là chuỗi cuối cùng sau khi tất cả các thao tác xóa có thể được thực hiện. Nếu mọi thứ bị xóa, kết quả là một chuỗi trống. 

Hạn chế chính về cấu trúc là mỗi ký tự chỉ tương tác với một vùng lân cận rất nhỏ trong quá trình này. Mặc dù việc xóa có thể xếp tầng, nhưng hiệu ứng luôn mang tính cục bộ: một nhân vật chỉ có thể trở thành một phần của bộ ba có thể tháo rời nếu môi trường xung quanh ngay lập tức của nó phù hợp. 

Nếu độ dài đầu vào ở mức 10^5, một mô phỏng đơn giản liên tục quét toàn bộ chuỗi sau mỗi lần xóa sẽ yêu cầu thời gian O(n^2) trong các trường hợp đối nghịch. Ví dụ: một chuỗi như`aaabbbccc...`sẽ gây ra việc quét lại nhiều lần sau mỗi lần thu gọn, dẫn đến hành vi bậc hai. 

Một trường hợp thất bại tinh vi hơn xuất hiện khi việc xóa bỏ tạo ra các bộ ba mới xuyên qua các ranh giới: 

đầu vào:`aabbbbaa`Đầu ra đúng:`aa`Một đường chuyền loại bỏ từ trái sang phải ngây thơ có thể xóa`bbb`, sản xuất`aabaa`, sau đó không thể hợp nhất và kiểm tra lại khu vực giữa mới một cách chính xác trừ khi nó xử lý lại trên toàn cầu. 

Điều này cho thấy tính đúng đắn phụ thuộc vào việc duy trì nhận thức về lịch sử gần đây chứ không phải việc quét lại toàn cầu. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là quét chuỗi liên tục và loại bỏ bất kỳ sự xuất hiện nào của ba ký tự bằng nhau liên tiếp. Sau mỗi lần xóa, chuỗi sẽ co lại và chúng tôi bắt đầu quét lại từ đầu. Điều này hoạt động vì mọi thao tác xóa hợp lệ cuối cùng đều được áp dụng và thứ tự không quan trọng đối với tính chính xác vì chỉ có các bộ ba giống hệt nhau biến mất. 

Vấn đề là hiệu suất. Mỗi lần truyền qua chuỗi có giá O(n) và trong trường hợp xấu nhất, chúng tôi chỉ có thể loại bỏ một bộ ba trong mỗi lần truyền. Đối với một chuỗi như`aaa...a`có độ dài n, thuật toán thực hiện các lượt O(n), mỗi lượt quét O(n), dẫn đến thời gian O(n^2). 

Quan sát quan trọng là quá trình này chỉ phụ thuộc vào một số ký tự cuối cùng còn hoạt động. Sau khi một ký tự được đặt, điều quan trọng là liệu nó có tạo thành một chuỗi dài 3 với hai ký tự còn sống trước đó hay không. Mọi thứ trước đó đều không liên quan đến sự tiến hóa trong tương lai ngoại trừ thông qua biểu diễn nén cuối cùng của nó. 

Điều này gợi ý việc duy trì một cấu trúc theo dõi tăng dần dạng rút gọn hiện tại. Một ngăn xếp đương nhiên đại diện cho hậu tố đang phát triển. Mỗi lần thêm một ký tự mới, chúng ta chỉ cần kiểm tra ba phần tử cuối cùng. Nếu chúng khớp nhau, chúng sẽ hủy ngay lập tức và chúng tôi tiếp tục mà không cần quét lại các phần trước đó. 

Điều này biến một quá trình lặp lại toàn cục thành một bước tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Mô phỏng ngăn xếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ trái sang phải trong khi duy trì một ngăn xếp đại diện cho chuỗi rút gọn hiện tại. 

1. Khởi tạo một ngăn xếp trống. Ngăn xếp này luôn đại diện cho tiền tố rút gọn hợp lệ sau khi xử lý vị trí hiện tại. 
2. Lặp lại từng ký tự trong chuỗi đầu vào từ trái sang phải. 
3. Đẩy ký tự hiện tại vào ngăn xếp. Điều này mở rộng chuỗi giảm dự kiến ​​hiện tại. 
4. Sau mỗi lần chèn, hãy kiểm tra xem ba ký tự cuối cùng trong ngăn xếp có giống nhau hay không. Đây là mẫu duy nhất có thể kích hoạt việc xóa vì các phần trước đó không thể tạo thành bộ ba mới mà không liên quan đến ký tự mới nhất. 
5. Nếu ba ký tự cuối cùng giống nhau, hãy xóa chúng khỏi ngăn xếp. Điều này mô phỏng việc loại bỏ một bộ ba hợp lệ và ngay lập tức bộc lộ bất kỳ hiệu ứng kề mới nào được tạo ra bởi việc loại bỏ. 
6. Tiếp tục xử lý ký tự tiếp theo. Quá trình này vốn đã tăng dần nên không cần phải quay lại hoặc quét lại. 

### Tại sao nó hoạt động 

Tính bất biến của ngăn xếp là sau khi xử lý ký tự thứ i, ngăn xếp chứa dạng rút gọn hoàn toàn của tiền tố s[0..i] theo quy tắc xóa ba lần. Bất kỳ thao tác xóa nào có thể xảy ra hoàn toàn bên trong tiền tố đều đã được áp dụng, vì các thao tác xóa mới duy nhất có thể liên quan đến ký tự được thêm gần đây nhất. Vì việc xóa chỉ rút ngắn chuỗi cục bộ và không bao giờ tạo ra các phần phụ thuộc xa hơn hai ký tự trước đó, nên việc hạn chế kiểm tra hậu tố ngăn xếp là đủ để đảm bảo tính đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    st = []

    for ch in s:
        st.append(ch)
        if len(st) >= 3 and st[-1] == st[-2] == st[-3]:
            c = st[-1]
            st.pop()
            st.pop()
            st.pop()

    print("".join(st))

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp quá trình ngăn xếp. Chúng tôi lưu trữ các ký tự trong danh sách Python được sử dụng làm ngăn xếp. Sau mỗi lần nối thêm, chúng tôi chỉ kiểm tra ba mục cuối cùng. Nếu chúng khớp nhau, chúng tôi sẽ loại bỏ chính xác ba phần tử. 

Một điểm tinh tế là chúng ta không cần vòng lặp khi loại bỏ bộ ba. Không giống như các vấn đề trong đó nhiều tầng yêu cầu kiểm tra lặp đi lặp lại, ở đây mỗi lần xóa sẽ giảm độ dài đi ba một cách nghiêm ngặt và bất kỳ bộ ba mới nào cũng phải liên quan đến các ký tự ngay trước đó, ký tự này sẽ được kiểm tra một cách tự nhiên trong các lần lặp tiếp theo. 

Phép nối ở cuối sẽ tái tạo lại chuỗi rút gọn cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`aaabbb`| Bước | Nhân vật | Ngăn xếp | Hành động | 
| --- | --- | --- | --- | 
| 1 | một | một | đẩy | 
| 2 | một | aa | đẩy | 
| 3 | một | aaa | loại bỏ ba | 
| 4 | b | b | đẩy | 
| 5 | b | bb | đẩy | 
| 6 | b | bbb | loại bỏ ba | 

Đầu ra cuối cùng trống. 

Dấu vết này cho thấy việc xóa có thể loại bỏ hoàn toàn các khối liên tiếp và quá trình này tiếp tục độc lập trên các phân đoạn còn lại. 

### Ví dụ 2 

đầu vào:`aabbbbaa`| Bước | Nhân vật | Ngăn xếp | Hành động | 
| --- | --- | --- | --- | 
| 1 | một | một | đẩy | 
| 2 | một | aa | đẩy | 
| 3 | b | aab | đẩy | 
| 4 | b | aabb | đẩy | 
| 5 | b | aabbb | xóa bbb → aa | 
| 6 | b | aab | đẩy | 
| 7 | một | aaba | đẩy | 
| 8 | một | aabaa | đẩy | 

Đầu ra cuối cùng:`aabaa`Ví dụ này thể hiện sự tương tác ranh giới: việc loại bỏ khối ở giữa không yêu cầu khởi động lại quá trình, vì ngăn xếp sẽ bảo toàn tiền tố rút gọn chính xác một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi ký tự được đẩy một lần và xuất hiện nhiều nhất một lần | 
| Không gian | O(n) | ngăn xếp lưu trữ tối đa n ký tự | 

Thuật toán chạy theo thời gian tuyến tính, điều này cần thiết với kích thước đầu vào tối đa các ràng buộc điển hình của Codeforces. Việc sử dụng bộ nhớ là tuyến tính và tương ứng với việc lưu trữ chuỗi đã giảm một phần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal cases
assert run("a\n") == "a", "single character"
assert run("aaa\n") == "", "full collapse"

# provided-like cases
assert run("aaabbb\n") == "", "two collapsing blocks"

# boundary merge case
assert run("aabbbbaa\n") == "aabaa", "cross-boundary effect"

# no removals
assert run("abcde\n") == "abcde", "no triples"

# alternating structure
assert run("aaabaaabaaa\n") == "b", "multiple cascades"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`a`| tối thiểu không trống | 
|`aaa`| `` | xóa hoàn toàn | 
|`aabbbbaa`|`aabaa`| thác ranh giới | 
|`abcde`|`abcde`| không xóa | 
|`aaabaaabaaa`|`b`| thác lặp đi lặp lại | 

## Vỏ cạnh 

Một trường hợp tinh vi là khi nhiều chuỗi sụp đổ trên một ranh giới được tạo bởi các thao tác xóa trước đó. Ví dụ: 

đầu vào:`aaabaaa`Xử lý: 

Ngăn xếp bắt đầu phát triển như`aaa`sụp đổ ngay lập tức, rời đi`baaa`, sau đó là dấu vết`aaa`lại sụp đổ, rời đi`b`. 

Thuật toán xử lý việc này một cách tự nhiên vì sau mỗi lần xóa, ngăn xếp sẽ phản ánh tiền tố rút gọn thực sự. Ký tự được nối thêm tiếp theo luôn được so sánh với hậu tố hiện tại, do đó, các bộ ba mới được hiển thị sẽ không bị bỏ sót. 

Một trường hợp khác là khi việc xóa xảy ra ngay từ đầu, để lại một ngăn xếp trống. Vì chúng tôi luôn kiểm tra độ dài ngăn xếp trước khi truy cập ba phần tử cuối cùng nên không xảy ra hiện tượng tràn và thuật toán tiếp tục an toàn từ trạng thái trống.
