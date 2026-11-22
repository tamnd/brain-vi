---
title: "CF 289B - Polo chim cánh cụt và ma trận"
description: "Chúng ta được cung cấp một lưới hình chữ nhật gồm các số nguyên và chúng ta được phép thực hiện một thao tác rất cụ thể: chọn một ô và cộng hoặc trừ một giá trị cố định d cho nó. Mỗi hoạt động như vậy tốn một lần di chuyển."
date: "2026-06-05T10:32:34+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "implementation", "sortings", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 289
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 177 (Div. 2)"
rating: 1400
weight: 289
solve_time_s: 65
verified: true
draft: false
---

[CF 289B - Polo the Penguin và Matrix](https://codeforces.com/problemset/problem/289/B) 

**Đánh giá:** 1400 
**Tags:** vũ lực, dp, triển khai, sắp xếp, tìm kiếm ternary 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới các số nguyên hình chữ nhật và chúng ta được phép thực hiện một thao tác rất cụ thể: chọn một ô và cộng hoặc trừ một giá trị cố định`d`đến nó. Mỗi hoạt động như vậy tốn một lần di chuyển. Mục tiêu là chuyển đổi toàn bộ lưới sao cho mỗi ô chứa chính xác cùng một số, sử dụng càng ít bước di chuyển càng tốt hoặc xác định rằng điều này là không thể. 

Điều quan trọng ngay từ đầu là thao tác không cho phép chúng ta thay đổi giá trị một cách tùy ý. Mỗi giá trị ô chỉ có thể di chuyển trong phạm vi modulo cấp số cộng của nó`d`. Nếu một giá trị bắt đầu tại`x`thì sau bao nhiêu thao tác nó cũng chỉ có thể trở thành số có dạng`x + k·d`. Điều này ngay lập tức ngụ ý một ràng buộc về cấu trúc: tất cả các ô phải đồng dạng modulo`d`, nếu không thì không có chuỗi thao tác nào có thể căn chỉnh chúng. 

Các ràng buộc rất nhỏ: tối đa là lưới 100 x 100, có thể lên tới 10.000 giá trị. Điều này cho phép một`O(nm log(nm))`tiếp cận một cách thoải mái vì việc sắp xếp hoặc quét tất cả các giá trị nhiều lần là rẻ. Ở đây bất cứ thứ gì bậc hai hoặc bậc ba trên mỗi ô đều ổn, nhưng chúng ta không cần bất cứ thứ gì gần như vậy. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Nếu tất cả các số không bằng nhau theo modulo`d`, câu trả lời ngay lập tức là không thể. Ví dụ, nếu`d = 2`và lưới chứa cả hai`1`Và`2`, sau đó`1`chỉ có thể đạt được số lẻ, trong khi`2`chỉ có thể đạt được số chẵn. Không có mục tiêu chung tồn tại. 

Nếu tất cả các giá trị đều bằng nhau thì câu trả lời là 0. Đây là việc nhỏ nhưng không nên bỏ qua trong quá trình thực hiện. 

Một trường hợp tinh tế khác là khi các giá trị khác nhau nhưng được căn chỉnh theo modulo`d`, Ví dụ`[1, 5, 9]`với`d = 4`. Tất cả đều tương đương với modulo 4, vì vậy vẫn tồn tại nghiệm mặc dù các giá trị cách xa nhau. Trực giác ngây thơ “làm cho mọi thứ bằng mức trung bình” sẽ thất bại ở đây bởi vì việc lấy trung bình là không thích hợp trong những chuyển động bị ràng buộc. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử mọi giá trị cuối cùng có thể và tính toán chi phí chuyển đổi tất cả các phần tử sang giá trị đó. Vì mỗi thao tác thay đổi một giá trị một cách chính xác`d`, mọi giá trị cuối cùng hợp lệ phải nằm trong cùng lớp modulo với tất cả các phần tử. Vì vậy, chúng tôi có thể chọn từng giá trị ứng cử viên từ lưới làm mục tiêu tiềm năng và tính toán tổng số bước cần thiết để chuyển đổi mọi ô sang mục tiêu đó. 

Đối với mục tiêu đã chọn`t`, mỗi phần tử`a[i][j]`đóng góp`|a[i][j] - t| / d`di chuyển, giả sử tính chia hết được giữ nguyên. Chúng tôi tính tổng số này trên tất cả các ô. Điều này mang lại sự chính xác vì mọi chuyển đổi đều độc lập. 

Tuy nhiên, có tới 10.000 ứng viên và có tới 10.000 ô, nên con số này trở thành khoảng 100 triệu thao tác, vẫn ở mức giới hạn nhưng không cần thiết. Quan trọng hơn, về cơ bản chúng ta đang lặp lại phép tính tương tự cho nhiều ứng viên tương đương. 

Thông tin chi tiết quan trọng là tất cả các giá trị đều nằm trong một lớp cấp tiến số học modulo`d`, vì vậy chúng ta có thể bình thường hóa chúng. Nếu chúng ta chia tất cả các giá trị cho`d`sau khi dịch chuyển một phần dư chung, bài toán giảm xuống còn việc chọn một mục tiêu số nguyên duy nhất để giảm thiểu độ lệch tuyệt đối. Đó là một bài toán cổ điển “cực tiểu hóa tổng các khác biệt tuyệt đối”, được giải quyết một cách tối ưu bằng số trung vị. 

Vì vậy, chúng tôi chuyển đổi từng giá trị như`b[i] = a[i] // d`(sau khi đảm bảo tính khả thi). Mục tiêu tối ưu là giá trị trung bình của`b`, vì tổng độ lệch tuyệt đối được giảm thiểu ở mức trung vị. Sau khi chọn được đường trung bình, câu trả lời chỉ đơn giản là tổng khoảng cách tới nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt mục tiêu | O(n²m²) | O(1) | Quá chậm | 
| Sắp xếp + giảm trung vị | O(nm log(nm)) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ma trận và làm phẳng tất cả các giá trị thành một danh sách. Điều này giúp đơn giản hóa việc lập luận vì chúng ta không còn cần cấu trúc hàng-cột nữa. 
2. Kiểm tra tính khả thi bằng cách đảm bảo tất cả các giá trị có cùng modulo dư`d`. Nếu thậm chí một giá trị khác nhau, chúng tôi sẽ trả về ngay lập tức`-1`bởi vì không có chuỗi hoạt động ±d nào có thể kết nối các lớp dư lượng khác nhau. 
3. Chuẩn hóa từng giá trị bằng cách chia cho`d`. Điều này chuyển đổi vấn đề thành các bước số nguyên trên một dòng, trong đó mỗi phép toán là ±1. 
4. Sắp xếp các giá trị chuẩn hóa. Việc sắp xếp là cần thiết vì điểm gặp tối ưu phụ thuộc vào thống kê thứ tự chứ không phải trung bình số học. 
5. Chọn giá trị trung bình làm mục tiêu. Điều này có hiệu quả vì việc di chuyển tất cả các điểm đến điểm trung vị sẽ giảm thiểu tổng độ lệch tuyệt đối trong mảng 1D. 
6. Tính tổng chi phí bằng cách cộng các chênh lệch tuyệt đối so với mức trung vị. 

### Tại sao nó hoạt động 

Mỗi lần di chuyển sẽ dịch chuyển một ô chính xác một đơn vị trong không gian chuẩn hóa, do đó mọi ô sẽ tiến hóa độc lập dọc theo một dòng. Tổng chi phí là tổng khoảng cách đến một điểm đã chọn. Trong một chiều, hàm được hình thành bởi độ lệch tuyệt đối là hàm lồi và tuyến tính từng phần, và mức tối thiểu của nó luôn nằm ở trung vị. Vì chúng tôi đã giảm lưới thành các điểm 1D độc lập theo một ràng buộc chung, nên không có sự tương tác nào giữa các ô ảnh hưởng đến mức tối ưu và việc chọn điểm trung bình trên toàn cầu sẽ giảm thiểu tổng số lần di chuyển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, d = map(int, input().split())
    vals = []

    for _ in range(n):
        row = list(map(int, input().split()))
        vals.extend(row)

    # feasibility check: all must share same mod d
    r = vals[0] % d
    for v in vals:
        if v % d != r:
            print(-1)
            return

    # normalize
    vals = [v // d for v in vals]
    vals.sort()

    median = vals[len(vals) // 2]

    ans = 0
    for v in vals:
        ans += abs(v - median)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách làm phẳng ma trận để mọi lý do có thể được thực hiện trên một mảng duy nhất. Việc kiểm tra modulo đảm bảo chúng ta không bao giờ thử những phép biến đổi không thể thực hiện được. Sự phân chia theo`d`giảm không gian thao tác sao cho mỗi bước di chuyển tương ứng với một bước đơn vị, điều này cần thiết để đối số trung vị được áp dụng một cách rõ ràng. 

Sắp xếp là bước quan trọng: nếu không sắp xếp thứ tự, chúng ta không thể xác định được số trung vị một cách hiệu quả. Vòng lặp cuối cùng tính tổng khoảng cách, tương ứng trực tiếp với số bước ±1 cần thiết trong không gian chuẩn hóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 2
2 4
6 8
```Giá trị làm phẳng:`[2, 4, 6, 8]`,`d = 2`Chuẩn hóa:`[1, 2, 3, 4]`| Bước | Mảng | Trung bình | Chi phí | 
| --- | --- | --- | --- | 
| Bình thường hóa | [1, 2, 3, 4] | - | - | 
| Sắp xếp | [1, 2, 3, 4] | - | - | 
| Chọn trung vị | - | 3 | - | 
| Chi phí tính toán | - | 3 | 2+1+0+1 = 4 | 

Đầu ra là`4`. 

Điều này xác nhận rằng sự phân bổ đồng đều xung quanh mức trung bình mang lại chi phí di chuyển tối thiểu. 

### Ví dụ 2 

đầu vào:```
3 3 3
3 6 9
12 15 18
21 24 27
```Làm phẳng:`[3,6,9,12,15,18,21,24,27]`Chuẩn hóa bởi`d=3`:`[1,2,3,4,5,6,7,8,9]`| Bước | Mảng | Trung bình | Chi phí | 
| --- | --- | --- | --- | 
| Bình thường hóa | [1..9] | - | - | 
| Trung bình | - | 5 | - | 
| Tổng khoảng cách | - | 5 | 20 | 

Điều này thể hiện tính đối xứng: các giá trị cách đều nhau xung quanh tâm tạo ra tổng chuyển động tối thiểu khi nhắm mục tiêu vào đường trung tuyến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm log(nm)) | thống trị bằng cách sắp xếp lưới phẳng | 
| Không gian | O(nm) | lưu trữ tất cả các giá trị trong một mảng | 

Các ràng buộc cho phép lên tới 10.000 giá trị, do đó việc sắp xếp và quét tuyến tính dễ dàng nằm trong giới hạn. Thuật toán tránh mọi hoạt động tính toán lại lồng nhau đối với các mục tiêu đề xuất, giữ cho thời gian chạy ổn định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline  # placeholder, replaced below

def solve():
    input = sys.stdin.readline
    n, m, d = map(int, input().split())
    vals = []
    for _ in range(n):
        vals.extend(map(int, input().split()))

    r = vals[0] % d
    for v in vals:
        if v % d != r:
            return "-1\n"

    vals = [v // d for v in vals]
    vals.sort()
    med = vals[len(vals)//2]

    return str(sum(abs(v - med) for v in vals)) + "\n"

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("2 2 2\n2 4\n6 8\n") == "4\n"

# all equal
assert run("2 2 3\n9 9\n9 9\n") == "0\n"

# impossible case
assert run("2 2 2\n1 2\n3 4\n") == "-1\n"

# single element
assert run("1 1 10\n5\n") == "0\n"

# larger spread
assert run("1 5 2\n2 4 6 8 10\n") == "6\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới nhất quán mod bằng 2x2 | 4 | tính đúng đắn cơ bản | 
| tất cả lưới bằng nhau | 0 | trường hợp nhận dạng tầm thường | 
| lưới chẵn lẻ hỗn hợp | -1 | kiểm tra tính khả thi | 
| ô đơn | 0 | trường hợp cạnh tối thiểu | 
| hàng cách đều nhau | 6 | hành vi trung bình | 

## Vỏ cạnh 

Một lưới hoàn toàn thống nhất như`[[7,7],[7,7]]`chứng minh rằng thuật toán đoản mạch một cách chính xác sau khi chuẩn hóa: tất cả các giá trị đều khớp, giá trị trung bình bằng giá trị và tổng chi phí bằng 0. 

Một lưới có dư lượng không tương thích như`[[1,2],[3,4]]`khi`d=2`gây ra sự từ chối ngay lập tức. Kiểm tra modulo nắm bắt được điều này trước khi sắp xếp, tránh tính toán không cần thiết. 

Lưới một ô cho thấy bước trung bình suy biến rõ ràng: phần tử duy nhất đã là mục tiêu và thuật toán trả về 0 một cách chính xác mà không cần viết hoa đặc biệt ngoài logic tiêu chuẩn.
