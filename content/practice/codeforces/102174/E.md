---
title: "CF 102174E - \u53ea\u6709\u4e00\u7aef\u5f00\u53e3\u7684\u74f6\u5b50"
description: "Chúng ta nhận được một hoán vị p[1..n] chứa mọi giá trị từ 1 đến n đúng một lần. Trình tự đầu vào chỉ có thể được sử dụng từ trái sang phải."
date: "2026-08-19T07:01:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102174
codeforces_index: "E"
codeforces_contest_name: "The 14-th BIT Campus Programming Contest"
rating: 0
weight: 102174
solve_time_s: 130
verified: true
draft: false
---

[CF 102174E - \u53ea\u6709\u4e00\u7aef\u5f00\u53e3\u7684\u74f6\u5b50](https://codeforces.com/problemset/problem/102174/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được một hoán vị`p[1..n]`chứa mọi giá trị từ`1`ĐẾN`n`đúng một lần. Trình tự đầu vào chỉ có thể được sử dụng từ trái sang phải. Mọi giá trị đã sử dụng có thể được đẩy lên một trong các ngăn xếp có sẵn và sau đó, giá trị trên cùng của ngăn xếp có thể được thêm vào câu trả lời hoặc được chuyển sang ngăn xếp khác. Đầu ra cuối cùng phải chính xác`1, 2, ..., n`. 

Nhiệm vụ là tìm số ngăn xếp nhỏ nhất có thể thực hiện được điều này. 

Điều ngạc nhiên chính là câu trả lời không bao giờ lớn hơn`2`. Với một ngăn xếp, một số hoán vị có thể đã được sắp xếp, trong khi mọi hoán vị không thể sắp xếp theo một ngăn xếp đều có thể được xử lý bằng hai ngăn xếp. Do đó, toàn bộ vấn đề giảm xuống còn việc nhận biết liệu hoán vị đã cho có phải là chuỗi đầu ra một ngăn hợp lệ hay không. 

Từ`n`có thể đạt được`10^5`, một thuật toán có hành vi bậc hai sẽ yêu cầu khoảng`10^10`các hoạt động cơ bản trong trường hợp xấu nhất, vượt xa giới hạn cuộc thi một giây có thể chịu đựng được. Chúng tôi cần một`O(n)`hoặc`O(n log n)`giải pháp. Thuộc tính hoán vị cũng có nghĩa là mọi giá trị là duy nhất, vì vậy chúng ta không bao giờ phải xử lý các phần tử bằng nhau như một trường hợp chính hãng. 

Có một số trường hợp nguy hiểm có thể đánh lừa một mô phỏng bất cẩn. Vì`n = 1`, hoán vị`[1]`chỉ cần một ngăn xếp, vì vậy câu trả lời là`1`. Việc triển khai ngây thơ giả định rằng ít nhất một phần tử phải nằm trong ngăn xếp có thể trả về không chính xác`2`. 

Đối với một hoán vị đã tăng dần như`1 2 3`, câu trả lời cũng là`1`. Mọi giá trị đều có thể được tiêu thụ và xuất ra ngay lập tức. Việc triển khai đẩy mọi giá trị đầu vào lên trước sẽ tạo ra trạng thái ngăn xếp một cách không cần thiết`[1, 2, 3]`, đỉnh của nó là`3`và có thể kết luận không chính xác rằng một ngăn xếp là không đủ. 

Mô hình thất bại quan trọng là`2 3 1`. Hai giá trị đầu tiên phải được lưu trữ trước`1`có thể được xuất ra, đưa ra trạng thái ngăn xếp`[2, 3]`. Sau đó`1`được sản xuất,`2`đang bị mắc kẹt bên dưới`3`, do đó một ngăn xếp không thể xuất ra`2`Kế tiếp. Câu trả lời đúng là`2`. Việc triển khai bất cẩn chỉ kiểm tra xem hoán vị có chứa cặp giảm hay chỉ liệu dãy con giảm dài nhất của nó có độ dài cụ thể nào đó hay không, có thể phân loại sai các trường hợp như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp có thể thử mọi số ngăn xếp có thể có và khám phá rõ ràng các hoạt động hợp pháp. Đối với cấu hình cố định, mỗi giá trị đầu vào có thể được gán cho các ngăn xếp khác nhau và giá trị cao nhất cũng có thể được di chuyển giữa các ngăn xếp trước khi xuất ra. Số lượng các chuỗi hoạt động có thể tăng theo cấp số nhân vì các giá trị giống nhau có thể được định tuyến qua các ngăn xếp khác nhau theo nhiều đơn hàng. Ngay cả việc hạn chế tìm kiếm ở tất cả các phép gán ngăn xếp có thể đã mang lại`k^n`khả năng cho`n`giá trị đầu vào và`k`ngăn xếp, điều đó thật vô vọng khi`n = 10^5`. 

Một cách tiếp cận ngây thơ hợp lý hơn là kiểm tra xem một ngăn xếp có hoạt động hay không bằng cách mô phỏng quy trình sắp xếp ngăn xếp tiêu chuẩn. Đầu ra mục tiêu bị ép buộc: giá trị bắt buộc tiếp theo luôn là`1`, sau đó`2`, sau đó`3`, vân vân. Khi giá trị đầu vào tiếp theo là giá trị yêu cầu thì chúng ta có thể xuất ra ngay lập tức. Nếu không thì nó phải được đẩy vào ngăn xếp. Bất cứ khi nào đỉnh ngăn xếp trở thành giá trị bắt buộc tiếp theo, chúng tôi sẽ bật nó lên. 

Quan sát quan trọng là quá trình tham lam này hoàn toàn xác định liệu một ngăn xếp có đủ hay không. Không bao giờ có lợi ích khi để giá trị được yêu cầu ở trên cùng của ngăn xếp, vì đầu ra phải tăng lên. Tương tự như vậy, nếu giá trị đầu vào tiếp theo là giá trị đầu ra được yêu cầu thì việc trì hoãn nó không có ích gì. 

Do đó, việc tìm kiếm bạo lực trên số lượng ngăn xếp có thể được giảm hơn nữa. Chúng ta chỉ cần phân biệt giữa một ngăn xếp và nhiều hơn một ngăn xếp. Nếu hoán vị vượt qua mô phỏng một ngăn, câu trả lời là`1`. Nếu không thì câu trả lời là`2`, vì hai ngăn xếp là đủ cho mọi hoán vị. Ngăn xếp thứ hai có thể được sử dụng làm nơi lưu trữ tạm thời khi ngăn xếp thứ nhất chứa phần tử chặn giá trị được yêu cầu tiếp theo. Đây chính xác là lý do tại sao mô hình hoạt động đa ngăn xếp có vẻ chung chung của vấn đề lại sụp đổ thành câu trả lời nhị phân. 

Điều kiện một ngăn xếp cũng tương đương với việc tránh kiểu hoán vị có thể sắp xếp ngăn xếp cổ điển`231`, nhưng việc triển khai mô phỏng đơn giản hơn và ít xảy ra lỗi hơn so với việc kiểm tra rõ ràng các mẫu bị cấm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm hoạt động đầy đủ | Hàm mũ | Hàm mũ | Quá chậm | 
| Hãy thử mọi số lượng ngăn xếp có thể |`O(n^2)`trong một mô phỏng đơn giản |`O(n)`| Quá chậm | 
| Mô phỏng một ngăn tham lam |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt`need = 1`, nghĩa là`1`là giá trị tiếp theo phải xuất hiện trong đầu ra được sắp xếp. Tạo một ngăn xếp phụ trống. Bản thân đầu ra không cần phải được lưu trữ vì chúng tôi chỉ quan tâm liệu mọi giá trị cần thiết có thể được tạo ra theo thứ tự hay không. 
2. Xử lý hoán vị từ trái sang phải. Nếu giá trị đầu vào hiện tại bằng`need`, tiêu thụ nó và tăng ngay lập tức`need`bởi một. Điều này là tối ưu vì giá trị đã chính xác là giá trị tiếp theo được yêu cầu bởi chuỗi tăng cuối cùng. 
3. Nếu giá trị đầu vào hiện tại không`need`, đẩy nó vào ngăn xếp. Nó chưa thể được xuất ra vì làm như vậy sẽ vi phạm thứ tự tăng dần cần thiết. 
4. Sau khi xử lý mọi giá trị đầu vào, hãy kiểm tra lại phần trên cùng của ngăn xếp. Trong khi đỉnh bằng`need`, bật nó lên và tăng lên`need`. Cửa sổ bật lên tham lam này bị ép buộc: giá trị cao nhất đã sẵn sàng để xuất ra và việc giữ nó ở đó không thể giúp sắp xếp trong tương lai tốt hơn. 
5. Rốt cuộc`n`giá trị đầu vào đã được sử dụng, hãy kiểm tra ngăn xếp. Nếu nó trống thì mọi giá trị đã được xuất ra theo thứ tự yêu cầu, vì vậy một ngăn xếp là đủ và câu trả lời là`1`. 
6. Nếu ngăn xếp không trống thì một ngăn xếp không thể sắp xếp hoán vị. Hai ngăn xếp là đủ, vì vậy câu trả lời là`2`. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý bất kỳ tiền tố nào của đầu vào, tất cả các giá trị nhỏ hơn`need`đã được xuất ra theo đúng thứ tự yêu cầu, trong khi mọi giá trị hiện được lưu trữ trong ngăn xếp đang chờ đến lượt. Bất cứ khi nào`need`nằm ở đầu ngăn xếp, việc xuất nó ngay lập tức là bắt buộc vì chuỗi cuối cùng không thể xuất ra bất cứ thứ gì lớn hơn trước. Bất cứ khi nào đầu vào hiện tại bằng`need`, việc đặt nó vào ngăn xếp trước tiên sẽ chỉ làm trì hoãn kết quả đầu ra chính xác đã có sẵn. 

Do đó, mô phỏng tham lam thành công chính xác khi tồn tại một thực thi một ngăn hợp pháp. Nếu nó bỏ lại các phần tử, thì các phần tử đó không thể được sắp xếp lại đủ bên trong một cấu trúc LIFO để tạo ra các giá trị còn thiếu theo thứ tự. Cấu trúc hai ngăn chung sau đó đưa ra giới hạn trên của`2`, do đó trường hợp một ngăn không thành công có câu trả lời tối thiểu chính xác`2`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        stack = []
        need = 1

        for x in p:
            if x == need:
                need += 1
            else:
                stack.append(x)

            while stack and stack[-1] == need:
                stack.pop()
                need += 1

        ans.append("1" if not stack else "2")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Biến`need`đại diện cho giá trị nhỏ nhất chưa được đặt vào chuỗi được sắp xếp cuối cùng. Nó bắt đầu lúc`1`và đạt tới`n + 1`sau khi xử lý thành công. 

Danh sách`stack`đại diện cho ngăn xếp duy nhất có sẵn trong thử nghiệm một ngăn xếp. Khi giá trị hoán vị hiện tại không thể sử dụng được ngay lập tức, nó sẽ được đẩy vào danh sách này. của Python`append`mô hình đẩy ngăn xếp, trong khi`pop`loại bỏ phần tử trên cùng. 

các`while`vòng lặp là cần thiết chứ không phải là một vòng lặp`if`. Một giá trị mới được sử dụng có thể hiển thị một giá trị bắt buộc khác bên dưới nó. Ví dụ, với hoán vị`3 2 1`, cả ba giá trị đều được đẩy và sau đó là chuỗi trên cùng`1, 2, 3`có thể được bật liên tiếp. Việc bỏ sót thao tác bật lên lặp đi lặp lại này là một lỗi thường gặp trong mô phỏng ngăn xếp. 

Bài kiểm tra dành cho`x == need`xảy ra trước khi đẩy`x`. Nếu giá trị đến chính xác là giá trị bắt buộc tiếp theo thì nó không bao giờ cần vào ngăn xếp. Đây là những gì cho phép hoán vị ngày càng tăng như`1 2 3`được xử lý bằng cách sử dụng một ngăn xếp mà không để lại bất kỳ thứ gì được lưu trữ. 

Không có vấn đề tràn số nguyên vì tất cả các giá trị nhiều nhất là`10^5`và số nguyên Python vẫn xử lý trực tiếp các bộ đếm. Tổng lượng dữ liệu được lưu trữ là tuyến tính theo`n`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với hoán vị`3 2 1`, quá trình mô phỏng ngăn xếp tiến hành như sau. 

| Giá trị đầu vào |`need`trước khi xử lý | Xếp chồng trước | Hành động |`need`sau | Xếp chồng sau | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 1 |`[]`| đẩy 3 | 1 |`[3]`| 
| 2 | 1 |`[3]`| đẩy 2 | 1 |`[3, 2]`| 
| 1 | 1 |`[3, 2]`| đẩy 1, rồi bật 1, 2, 3 | 4 |`[]`| 

Khi`1`đến, nó được đẩy vì mô phỏng xử lý mọi giá trị không ngay lập tức một cách thống nhất. Nó trở thành đỉnh của ngăn xếp, vì vậy`1`được bật lên. Điều đó bộc lộ`2`, đây cũng là giá trị bắt buộc tiếp theo, vì vậy`2`cũng được bật lên. Cuối cùng`3`được phơi bày và bật ra. 

Ngăn xếp trống ở cuối nên kết quả là`1`. Ví dụ này chứng minh tại sao việc lặp đi lặp lại`while`vòng lặp là cần thiết. 

### Mẫu 2 

Đối với hoán vị`2 1`, vết là: 

| Giá trị đầu vào |`need`trước khi xử lý | Xếp chồng trước | Hành động |`need`sau | Xếp chồng sau | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 1 |`[]`| đẩy 2 | 1 |`[2]`| 
| 1 | 1 |`[2]`| đẩy 1, sau đó bật 1 và 2 | 3 |`[]`| 

Sau đó`1`được đẩy, nó có thể bật ra ngay lập tức. Điều đó bộc lộ`2`, bây giờ chính xác là giá trị bắt buộc tiếp theo. Do đó, cả hai giá trị đều được tạo theo thứ tự được sắp xếp bằng cách sử dụng một ngăn xếp. 

Ngăn xếp cuối cùng trống, đưa ra câu trả lời`1`. 

### Mẫu 3 

Đối với hoán vị`2 3 1`, vết là: 

| Giá trị đầu vào |`need`trước khi xử lý | Xếp chồng trước | Hành động |`need`sau | Xếp chồng sau | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 1 |`[]`| đẩy 2 | 1 |`[2]`| 
| 3 | 1 |`[2]`| đẩy 3 | 1 |`[2, 3]`| 
| 1 | 1 |`[2, 3]`| đẩy 1, bật 1 | 2 |`[2, 3]`| 

Sau đó`1`là đầu ra, giá trị bắt buộc tiếp theo là`2`, nhưng đỉnh ngăn xếp là`3`. Từ`2`đang bị mắc kẹt bên dưới`3`, một ngăn xếp không thể tiếp tục. 

Ngăn xếp còn lại chứng tỏ rằng câu trả lời là không`1`. Hai ngăn xếp là đủ, vì vậy câu trả lời là`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`mỗi trường hợp thử nghiệm | Mỗi giá trị đầu vào được đẩy nhiều nhất một lần và xuất hiện nhiều nhất một lần. | 
| Không gian |`O(n)`| Ngăn xếp phụ có thể chứa tất cả`n`các giá trị. | 

Trong tất cả các trường hợp thử nghiệm, giới hạn dự định tự nhiên là tuyến tính trong tổng số phần tử hoán vị. Với`n`lên đến`10^5`, điều này dễ dàng phù hợp với các giới hạn cần thiết vì thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi giá trị đầu vào. 

## Trường hợp thử nghiệm 

Vấn đề ban đầu đảm bảo rằng mọi đầu vào đều là một hoán vị, do đó phép thử "tất cả các giá trị bằng nhau" không phải là một trường hợp thử nghiệm hợp lệ. Kiểm tra ranh giới có ý nghĩa gần nhất là một hoán vị tăng nghiêm ngặt, trong đó mọi giá trị có thể được xuất ra ngay lập tức.```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        stack = []
        need = 1

        for x in p:
            if x == need:
                need += 1
            else:
                stack.append(x)

            while stack and stack[-1] == need:
                stack.pop()
                need += 1

        ans.append("1" if not stack else "2")

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Provided samples
assert run(
    "3\n"
    "3\n"
    "3 2 1\n"
    "2\n"
    "2 1\n"
    "3\n"
    "2 3 1\n"
) == "1\n1\n2", "provided samples"

# Minimum-size input
assert run(
    "1\n"
    "1\n"
    "1\n"
) == "1", "single element"

# Already sorted, every value can be output immediately
assert run(
    "1\n"
    "5\n"
    "1 2 3 4 5\n"
) == "1", "increasing permutation"

# A classic one-stack failure
assert run(
    "1\n"
    "4\n"
    "2 3 1 4\n"
) == "2", "231 pattern"

# Decreasing permutation, requiring repeated pops
assert run(
    "1\n"
    "6\n"
    "6 5 4 3 2 1\n"
) == "1", "maximum stack depth"

# Large boundary case, increasing permutation
n = 100000
p = " ".join(map(str, range(1, n + 1)))
assert run(f"1\n{n}\n{p}\n") == "1", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1`|`1`| Đầu vào nhỏ nhất có thể và khởi tạo ranh giới của`need`. | 
|`1 / 5 / 1 2 3 4 5`|`1`| Các giá trị có thể được xuất trực tiếp mà không cần sử dụng ngăn xếp. | 
|`1 / 4 / 2 3 1 4`|`2`| Phát hiện`231`tắc nghẽn và ngăn chặn kết quả một ngăn xếp sai. | 
|`1 / 6 / 6 5 4 3 2 1`|`1`| Thực hiện độ sâu ngăn xếp tối đa và bật lên lặp đi lặp lại. | 
|`1 / 100000 / 1 2 ... 100000`|`1`| Xác nhận hành vi tuyến tính ở mức tối đa cho phép`n`. | 

## Vỏ cạnh 

cho`n = 1`, đầu vào chính xác là`1`. Quá trình mô phỏng bắt đầu với`need = 1`, thấy rằng giá trị hiện tại bằng`need`, và số gia`need`ĐẾN`2`. Ngăn xếp vẫn trống, vì vậy câu trả lời là`1`. 

Đối với một hoán vị đã được sắp xếp như`1 2 3 4 5`, mọi giá trị đến đều bằng`need`. Thuật toán không bao giờ đẩy bất cứ điều gì, và`need`tiến bộ từ`1`ĐẾN`6`. Ngăn xếp cuối cùng trống, vì vậy câu trả lời là`1`. Điều này nắm bắt các triển khai đẩy mọi giá trị đầu vào một cách không cần thiết trước khi cố gắng bật. 

Đối với sự cản trở`2 3 1`, hai giá trị đầu tiên được đẩy, tạo ra`[2, 3]`. Khi`1`đến, nó được đẩy và ngay lập tức bật ra, rời đi`[2, 3]`với`3`ở trên cùng trong khi`2`được yêu cầu. Ngăn xếp không thể tạo ra giá trị tiếp theo chính xác, do đó mô phỏng kết thúc với bộ nhớ không trống và trả về`2`. 

Đối với hoán vị giảm dần`6 5 4 3 2 1`, mọi giá trị ban đầu được đẩy. Một lần`1`đến, các`while`vòng lặp loại bỏ`1`, sau đó`2`, sau đó`3`, sau đó`4`, sau đó`5`, và cuối cùng`6`. Ngăn xếp trở nên trống rỗng, chứng tỏ rằng bản thân ngăn xếp sâu không phải là lý do để yêu cầu nhiều ngăn xếp. Thứ tự của các giá trị bị mắc kẹt rất quan trọng. 

Để có kích thước đầu vào tối đa, hoán vị chiều dài ngày càng tăng`100000`gây ra chính xác một quyết định theo thời gian không đổi cho mỗi giá trị và không bao giờ tăng ngăn xếp. Hoán vị giảm dần có cùng kích thước sẽ đẩy mọi giá trị và sau đó bật lên mọi giá trị một lần. Trong cả hai trường hợp, tổng số thao tác ngăn xếp vẫn tuyến tính, do đó việc triển khai vẫn nằm trong giới hạn độ phức tạp dự kiến.
