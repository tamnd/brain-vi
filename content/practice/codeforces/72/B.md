---
title: "CF 72B - tệp INI"
description: "Chúng tôi được cung cấp một tệp văn bản được viết ở định dạng INI. Mỗi dòng có ý nghĩa đều là một khai báo phần chẳng hạn như [mạng] hoặc gán khóa-giá trị chẳng hạn như port=8080. Các khoảng trắng xung quanh khóa, giá trị và dấu ngoặc phần không liên quan và phải được loại bỏ trong kết quả cuối cùng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 72
codeforces_index: "B"
codeforces_contest_name: "Unknown Language Round 2"
rating: 2200
weight: 72
solve_time_s: 105
verified: true
draft: false
---

[CF 72B - tệp INI](https://codeforces.com/problemset/problem/72/B) 

**Đánh giá:** 2200 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 45 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tệp văn bản được viết ở định dạng INI. Mỗi dòng có ý nghĩa là một phần khai báo, chẳng hạn như`[network]`hoặc gán khóa-giá trị, chẳng hạn như`port=8080`. Các khoảng trắng xung quanh khóa, giá trị và dấu ngoặc phần không liên quan và phải được loại bỏ trong kết quả cuối cùng. Các dòng có ký tự không phải khoảng trắng đầu tiên là`;`là những bình luận và nên được bỏ qua hoàn toàn. 

Tệp hoạt động giống như một chuỗi các bản cập nhật. Khóa thuộc về phần hiện tại hoặc thuộc phạm vi toàn cầu nếu chưa có phần nào được khai báo. Nếu cùng một khóa xuất hiện nhiều lần trong cùng một phần thì chỉ lần xuất hiện cuối cùng còn tồn tại. Các phần khác nhau là độc lập, do đó, cùng một tên khóa có thể tồn tại trong một số phần. 

Đầu ra phải được chuẩn hóa theo ba cách. Chìa khóa toàn cầu được ưu tiên hàng đầu. Các phần được in sau đó theo thứ tự từ điển. Bên trong mỗi phạm vi, các khóa cũng được in theo thứ tự từ điển. Các dòng trống, nhận xét và khoảng trống thừa sẽ biến mất hoàn toàn. 

Các ràng buộc là rất nhỏ so với hầu hết các vấn đề lập trình cạnh tranh. Có tối đa 510 dòng và tổng kích thước đầu vào chỉ là 10000 ký tự. Ngay cả các chiến lược phân tích cú pháp tương đối kém hiệu quả cũng có thể vượt qua một cách thoải mái. Thách thức chính không phải là hiệu suất mà là tái tạo chính xác ngữ nghĩa chính xác của việc ghi đè, cắt bớt khoảng trắng và xử lý phần. 

Một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. 

Một phần có thể xuất hiện nhiều lần trong tệp. Tất cả các khóa thuộc phần đó phải được hợp nhất với nhau. 

đầu vào:```
4
[a]
x=1
[a]
y=2
```Đầu ra đúng:```
[a]
x=1
y=2
```Một cách triển khai đơn giản tạo ra một đối tượng phần mới mỗi lần`[a]`xuất hiện sẽ in không chính xác hai phần riêng biệt. 

Khóa phải ghi đè lên các giá trị trước đó trong cùng một phần. 

đầu vào:```
4
[a]
x=1
x=7
y=2
```Đầu ra đúng:```
[a]
x=7
y=2
```Nếu chúng ta chỉ thêm các bài tập vào một danh sách, cả hai`x`dòng sẽ vẫn còn. 

Xử lý khoảng trắng cũng tinh tế. 

đầu vào:```
3
  a   =   5
[b]
   c =  7
```Đầu ra đúng:```
a=5
[b]
c=7
```Nếu các khoảng trống không được loại bỏ cẩn thận xung quanh cả hai mặt của`=`, định dạng đầu ra trở nên không hợp lệ. 

Phát hiện nhận xét phụ thuộc vào ký tự không phải khoảng trắng đầu tiên. 

đầu vào:```
2
   ;hello
x=1
```Đầu ra đúng:```
x=1
```Chỉ kiểm tra`line[0] == ';'`sẽ thất bại vì dấu chấm phẩy không phải là ký tự đầu tiên theo đúng nghĩa đen. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng tệp chính xác như được viết. Chúng tôi quét mọi dòng, duy trì phần hiện tại và lưu trữ các cặp khóa-giá trị trong cấu trúc được liên kết với phần đó. Bất cứ khi nào chúng tôi gặp khóa trùng lặp, chúng tôi sẽ ghi đè giá trị cũ. Sau khi phân tích cú pháp xong, chúng tôi sắp xếp các phần và khóa trước khi in. 

Việc triển khai thực sự mạnh mẽ có thể lưu trữ mọi nhiệm vụ trong danh sách và trong quá trình xuất, quét ngược để tìm giá trị cuối cùng cho mỗi khóa. Nếu có`m`bài tập, điều này có thể yêu cầu phải kiểm tra lại hầu hết mọi bài tập trước đó. Trong trường hợp xấu nhất, điều này trở thành`O(m^2)`hoạt động. 

Cách tiếp cận đó vẫn được áp dụng ở đây vì đầu vào nhỏ nhưng phức tạp và dễ xảy ra lỗi một cách không cần thiết. Cấu trúc của bài toán cho chúng ta một quan sát rõ ràng hơn: chỉ giá trị mới nhất cho mỗi`(section, key)`vấn đề cặp đôi. Các bài tập trước đó sẽ trở nên không liên quan ngay sau khi việc ghi đè sau đó xuất hiện. 

Quan sát đó tự nhiên dẫn đến bản đồ băm. Chúng tôi duy trì một từ điển cho mỗi phần. Việc gán một phím chỉ đơn giản trở thành:```
sections[current][key] = value
```Mọi giá trị trước đó sẽ tự động được thay thế. Sau khi phân tích cú pháp xong, chúng ta chỉ cần sắp xếp từ điển để định dạng đầu ra. 

Bản thân việc phân tích cú pháp chủ yếu là công việc triển khai. Chúng ta phải phân biệt giữa các nhận xét, tiêu đề phần và bài tập trong khi cắt bớt khoảng trắng một cách chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m2) | O(m) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(m log m) | O(m) | Đã chấp nhận | 

Đây,`m`là số lượng khóa được lưu trữ riêng biệt cộng với chi phí sắp xếp. 

## Hướng dẫn thuật toán 

1. Tạo một từ điển cho các khóa chung và một từ điển khác ánh xạ tên các phần vào từ điển của riêng chúng. 
2. Khởi tạo phần hiện tại làm phạm vi toàn cục. Chúng ta có thể biểu diễn nó bằng một chuỗi rỗng. 
3. Đọc từng dòng và chỉ loại bỏ ký tự dòng mới ở cuối. 
4. Xóa khoảng trắng ở đầu và cuối dòng. Điều này đơn giản hóa việc kiểm tra sau này. 
5. Nếu dòng trống sau khi cắt, hãy bỏ qua nó. Tuyên bố cho biết các dòng thừa sẽ biến mất. 
6. Nếu ký tự đầu tiên là`;`, bỏ qua dòng này vì đó là một nhận xét. 
7. Nếu dòng đại diện cho tiêu đề phần, hãy trích xuất tên phần. 

Một dòng phần bắt đầu bằng`[`và kết thúc bằng`]`sau khi cắt bớt không gian. Chúng tôi loại bỏ các dấu ngoặc và cắt bớt bên trong một lần nữa vì khoảng trắng xung quanh dấu ngoặc không liên quan. 
8. Nếu phần này chưa xuất hiện trước đó, hãy tạo một từ điển trống cho nó. 
9. Cập nhật phần hiện tại thành tên phần này. 
10. Nếu không, dòng này phải là một phép gán khóa-giá trị. Tách chuỗi lúc đầu`=`tính cách. 

Việc tách chỉ một lần là quan trọng vì bản thân các giá trị có thể chứa các ký hiệu không được chạm tới. 
11. Cắt riêng các khoảng trống xung quanh khóa và giá trị. 
12. Lưu bài tập vào từ điển của phần hiện tại. 

Nếu khóa đã tồn tại ở đó, giá trị mới sẽ tự động ghi đè giá trị cũ. 
13. Sau khi xử lý tất cả các dòng, trước tiên hãy in các khóa chung theo thứ tự được sắp xếp. 
14. Sau đó in tất cả các phần theo thứ tự từ điển được sắp xếp. Đối với mỗi phần, hãy in các khóa của nó theo thứ tự được sắp xếp. 

### Tại sao nó hoạt động 

Tại mọi thời điểm trong quá trình phân tích cú pháp, mỗi từ điển đều lưu trữ chính xác giá trị mới nhất được thấy cho mọi khóa trong phạm vi đó. Khi một phép gán trùng lặp xuất hiện, việc thay thế giá trị được lưu trữ sẽ phù hợp với yêu cầu của vấn đề là chỉ lần xuất hiện cuối cùng mới tồn tại. 

Các phần độc lập vì mỗi phần có từ điển riêng. Sự xuất hiện lặp đi lặp lại của cùng một phần sẽ tiếp tục sửa đổi cùng một từ điển, kết hợp chính xác các định nghĩa rải rác trong toàn bộ tệp. 

Bước sắp xếp cuối cùng đảm bảo thứ tự từ điển bất kể thứ tự đầu vào ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    sections = {"": {}}
    current = ""

    for _ in range(n):
        line = input().rstrip("\n").strip()

        if not line:
            continue

        if line[0] == ';':
            continue

        if line[0] == '[' and line[-1] == ']':
            section = line[1:-1].strip()

            if section not in sections:
                sections[section] = {}

            current = section
        else:
            key, value = line.split("=", 1)

            key = key.strip()
            value = value.strip()

            sections[current][key] = value

    global_keys = sections[""]

    for key in sorted(global_keys):
        print(f"{key}={global_keys[key]}")

    for section in sorted(s for s in sections if s != ""):
        print(f"[{section}]")

        for key in sorted(sections[section]):
            print(f"{key}={sections[section][key]}")

if __name__ == "__main__":
    solve()
```Giải pháp giữ mọi phạm vi trong từ điển. Chuỗi trống biểu thị phạm vi toàn cục vì các khóa trước phần đầu tiên không thuộc về nơi nào khác. 

Logic phân tích cú pháp bắt đầu bằng cách cắt bớt toàn bộ dòng. Điều này rất quan trọng vì các nhận xét có thể bắt đầu sau các khoảng trắng ở đầu và các dấu ngoặc phần cũng có thể chứa các khoảng trắng xung quanh. 

Phần phân tích cú pháp sử dụng:```
section = line[1:-1].strip()
```Không có thứ hai`strip()`, một đầu vào như`[  abc  ]`sẽ lưu trữ không chính xác tên phần có dấu cách. 

Bài tập sử dụng:```
line.split("=", 1)
```Đối số thứ hai ngăn chặn việc chia tách thêm một cách ngẫu nhiên. Mặc dù các ràng buộc chính thức giữ cho các giá trị đơn giản nhưng đây là kiểu phân tích cú pháp an toàn nhất. 

Hành vi ghi đè xuất phát tự nhiên từ việc gán từ điển. Không cần xử lý trùng lặp rõ ràng. 

Trong quá trình xuất, phạm vi toàn cục được in trước, sau đó là từng phần thực theo thứ tự được sắp xếp. Các khóa bên trong mỗi phạm vi cũng được sắp xếp trước khi in. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
11
a= 1
b=a
a = 2
 ; comment
[z]
1=2
[y]
2=3
[z]
2=1
[w]
```Dấu vết: 

| Bước | Dòng | Phần hiện tại | Hành động | Trạng thái lưu trữ | 
| --- | --- | --- | --- | --- | 
| 1 |`a= 1`| toàn cầu | cửa hàng`a=1`| toàn cầu:`{a:1}`| 
| 2 |`b=a`| toàn cầu | cửa hàng`b=a`| toàn cầu:`{a:1,b:a}`| 
| 3 |`a = 2`| toàn cầu | ghi đè lên`a`| toàn cầu:`{a:2,b:a}`| 
| 4 |`; comment`| toàn cầu | bỏ qua | không thay đổi | 
| 5 |`[z]`| z | tạo phần | z:`{}`| 
| 6 |`1=2`| z | cửa hàng`1=2`| z:`{1:2}`| 
| 7 |`[y]`| y | tạo phần | y:`{}`| 
| 8 |`2=3`| y | cửa hàng`2=3`| y:`{2:3}`| 
| 9 |`[z]`| z | phần chuyển đổi | không thay đổi | 
| 10 |`2=1`| z | cửa hàng`2=1`| z:`{1:2,2:1}`| 
| 11 |`[w]`| w | tạo phần trống | w:`{}`| 

Đầu ra cuối cùng:```
a=2
b=a
[w]
[y]
2=3
[z]
1=2
2=1
```Dấu vết này cho thấy hai thuộc tính quan trọng. Phần nhập lại`z`tiếp tục sửa đổi cùng một từ điển và các khóa chung trùng lặp sẽ ghi đè lên các giá trị trước đó. 

### Ví dụ tùy chỉnh 

đầu vào:```
8
  ;ignore
[a]
x=1
x = 9
[b]
z=3
[a]
y=2
```Dấu vết: 

| Bước | Dòng | Phần hiện tại | Hành động | Tiểu bang | 
| --- | --- | --- | --- | --- | 
| 1 |`;ignore`| toàn cầu | bỏ qua | trống | 
| 2 |`[a]`| một | tạo phần | Một:`{}`| 
| 3 |`x=1`| một | cửa hàng | Một:`{x:1}`| 
| 4 |`x = 9`| một | ghi đè | Một:`{x:9}`| 
| 5 |`[b]`| b | tạo phần | b:`{}`| 
| 6 |`z=3`| b | cửa hàng | b:`{z:3}`| 
| 7 |`[a]`| một | chuyển lại | không thay đổi | 
| 8 |`y=2`| một | cửa hàng | Một:`{x:9,y:2}`| 

Đầu ra cuối cùng:```
[a]
x=9
y=2
[b]
z=3
```Ví dụ này chứng tỏ rằng các phần lặp lại sẽ hợp nhất một cách tự nhiên và việc ghi đè chỉ xảy ra trong một phần duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Việc sắp xếp các phím và tên phần chiếm ưu thế | 
| Không gian | O(m) | Lưu trữ tất cả các cặp khóa-giá trị còn sót lại | 

Đây,`m`là số lượng bài tập được lưu trữ sau khi phân tích cú pháp. Vì tổng kích thước đầu vào chỉ có 10000 ký tự nên giải pháp này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    sections = {"": {}}
    current = ""

    for _ in range(n):
        line = input().rstrip("\n").strip()

        if not line:
            continue

        if line[0] == ';':
            continue

        if line[0] == '[' and line[-1] == ']':
            section = line[1:-1].strip()

            if section not in sections:
                sections[section] = {}

            current = section
        else:
            key, value = line.split("=", 1)

            key = key.strip()
            value = value.strip()

            sections[current][key] = value

    out = []

    for key in sorted(sections[""]):
        out.append(f"{key}={sections[''][key]}")

    for section in sorted(s for s in sections if s != ""):
        out.append(f"[{section}]")

        for key in sorted(sections[section]):
            out.append(f"{key}={sections[section][key]}")

    print("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

# provided sample
assert run(
"""11
a= 1
b=a
a = 2
 ; comment
[z]
1=2
[y]
2=3
[z]
2=1
[w]
"""
) == """a=2
b=a
[w]
[y]
2=3
[z]
1=2
2=1""", "sample 1"

# minimum case
assert run(
"""1
a=1
"""
) == """a=1""", "single global key"

# duplicate overwrite
assert run(
"""4
[a]
x=1
x=2
x=3
"""
) == """[a]
x=3""", "latest value survives"

# repeated sections merge
assert run(
"""5
[a]
x=1
[b]
y=2
[a]
"""
) == """[a]
x=1
[b]
y=2""", "same section reused"

# whitespace and comments
assert run(
"""5
   ; ignore
 [ sec ]
  a   =   9
b=2
 ;x
"""
) == """[sec]
a=9
b=2""", "space trimming"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chìa khóa đơn |`a=1`| Đầu vào hợp lệ tối thiểu | 
| Bài tập trùng lặp |`x=3`sống sót | Ghi đè ngữ nghĩa | 
| Phần lặp đi lặp lại | Các phím được hợp nhất vào cùng một phần | Tái sử dụng phần đúng | 
| Đầu vào nặng khoảng trắng | Làm sạch định dạng chuẩn hóa | Cắt tỉa và xử lý nhận xét đúng cách | 

## Vỏ cạnh 

Xem xét các khai báo phần lặp đi lặp lại. 

đầu vào:```
5
[a]
x=1
[b]
y=2
[a]
```Khi trình phân tích cú pháp nhìn thấy thứ hai`[a]`, nó không tạo từ điển mới vì phần`a`đã tồn tại rồi. Nó chỉ chuyển mạch`current`quay lại`"a"`. Dữ liệu được lưu trữ vẫn còn:```
a -> {x:1}
b -> {y:2}
```Đầu ra cuối cùng chứa chính xác một`[a]`phần. 

Bây giờ hãy xem xét việc ghi đè bên trong một phần. 

đầu vào:```
4
[a]
x=1
x=5
x=9
```Các chuyển đổi từ điển là:```
{x:1}
{x:5}
{x:9}
```Mỗi phép gán sẽ thay thế giá trị trước đó. Chỉ có cái mới nhất còn sót lại, khớp chính xác với đặc điểm kỹ thuật. 

Phân tích cú pháp nặng về khoảng trắng là một nguồn lỗi phổ biến khác. 

đầu vào:```
3
 [ abc ]
   k   =   v
 ;hello
```Sau khi cắt tỉa:```
[ abc ]
k   =   v
;hello
```Tên phần trở thành`"abc"`sau khi tháo dấu ngoặc và tước lại. Nhiệm vụ trở thành`k=v`. Bình luận biến mất hoàn toàn. 

Cuối cùng, các khóa chung phải tách biệt với các khóa phần. 

đầu vào:```
4
x=1
[a]
x=2
y=3
```Trình phân tích cú pháp lưu trữ:```
global -> {x:1}
a -> {x:2, y:3}
```hai`x`các khóa không xung đột vì chúng thuộc các phạm vi khác nhau. Đầu ra là:```
x=1
[a]
x=2
y=3
```
