---
title: "CF 102396A - Thanh tra của nhà vua"
description: "Chúng ta có ba rương chứa các đồng xu a, b và c. Trong một giây, chúng tôi chọn chính xác hai rương khác nhau và thêm một đồng xu vào mỗi rương đã chọn. Chúng tôi cần cả ba rương kết thúc với cùng một số xu và chúng tôi muốn có số giây tối thiểu."
date: "2026-08-10T18:31:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102396
codeforces_index: "A"
codeforces_contest_name: "2019-2020 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 19)"
rating: 0
weight: 102396
solve_time_s: 793
verified: true
draft: false
---

[CF 102396A - Cuộc thanh tra của nhà vua](https://codeforces.com/problemset/problem/102396/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13m 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có ba cái rương chứa`a`,`b`, Và`c`tiền xu. Trong một giây, chúng tôi chọn chính xác hai rương khác nhau và thêm một đồng xu vào mỗi rương đã chọn. Chúng tôi cần cả ba rương kết thúc với cùng một số xu và chúng tôi muốn có số giây tối thiểu. 

Đầu vào chứa ba số tiền xu ban đầu, mỗi số nằm giữa`1`Và`5 * 10^8`. Chỉ có ba giá trị liên quan nên không cần cấu trúc dữ liệu phức tạp. Giới hạn trên lớn là tín hiệu thực: một mô phỏng thực hiện một thao tác mỗi giây có thể yêu cầu gần một tỷ lần lặp, vượt xa giới hạn 1 giây cho phép. Một giải pháp theo thời gian không đổi hoặc logarit là mục tiêu thích hợp. 

Trường hợp chính là khi rương lớn nhất đã có giá trị mục tiêu mà chúng ta có thể xem xét ban đầu. Ví dụ, với```
1
2
3
```một cách tiếp cận bất cẩn có thể cố gắng nâng hai rương đầu tiên lên`3`. Những thâm hụt là`2`Và`1`, trong khi rương thứ ba không cần xu. Vì mọi thao tác đều ảnh hưởng đến hai rương nên hai rương đầu tiên không thể tăng số lượng khác nhau một cách độc lập trong khi vẫn giữ nguyên rương thứ ba. Câu trả lời đúng là`3`, đạt`4, 4, 4`. 

Một trường hợp cạnh khác là một cấu hình đã bằng nhau:```
2
2
2
```Câu trả lời là`0`. Một mô phỏng luôn thực hiện ít nhất một thao tác trước khi kiểm tra đẳng thức sẽ trả về giá trị dương không chính xác. 

Các giá trị lặp đi lặp lại cũng đáng được quan tâm. Vì```
1
3
3
```câu trả lời là`4`, không`2`. Nâng rương đầu tiên bằng`2`vẫn chưa đủ vì mỗi thao tác cũng nâng lên một trong hai rương còn lại. Giá trị cuối cùng đúng là`5`, yêu cầu bốn thao tác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng các hoạt động từng giây một. Ở mỗi bước, chúng ta có thể chọn một cặp rương giúp chúng ta hướng tới sự bình đẳng, cập nhật giá trị của chúng và dừng lại khi cả ba giá trị đều khớp. Điều này đúng nếu cặp được chọn phù hợp, nhưng bản thân số giây có thể rất lớn. 

Sau khi sắp xếp các giá trị như`x <= y <= z`, trường hợp xấu nhất theo các ràng buộc là`x = 1`,`y = z = 5 * 10^8`. Vậy thì câu trả lời tối thiểu là`y + z - 2x = 999,999,998`. 

Do đó, một mô phỏng sẽ thực hiện gần một tỷ lần lặp. Mặc dù mỗi lần lặp có thời gian không đổi nhưng như vậy là quá nhiều so với giới hạn thời gian. 

Quan sát quan trọng là một thao tác luôn cộng tổng cộng chính xác hai đồng xu. Thay vì quyết định chọn cặp nào ở mỗi giây, chúng ta có thể suy luận xem mỗi rương phải nhận được bao nhiêu xu ở trạng thái cuối cùng. 

Giả sử giá trị chung cuối cùng là`T`. Ba rương cần`T-x`,`T-y`, Và`T-z`tiền bổ sung. Vì mỗi thao tác sẽ mang lại một xu cho hai rương nên mức tăng yêu cầu phải có thể ghép đôi được. Đặc biệt, mức tăng yêu cầu lớn nhất không được vượt quá tổng của hai mức tăng còn lại. 

Đối với rương nhỏ nhất, mức tăng yêu cầu là`T-x`, là lớn nhất trong ba. Vì vậy chúng ta cần`T - x <= (T - y) + (T - z)`. 

Sắp xếp lại mang lại`T >= y + z - x`. 

Vậy giá trị chung nhỏ nhất có thể chính xác là`T = y + z - x`. 

Mục tiêu này luôn có thể đạt được. Sự gia tăng yêu cầu của nó trở thành`y + z - 2x`,`z - x`, Và`y - x`. 

Đồng xu đầu tiên chính xác là tổng của hai đồng xu còn lại, vì vậy mỗi đồng xu được trao cho rương nhỏ nhất có thể ghép với đồng xu được trao cho một trong các rương khác. 

Vì mỗi thao tác thêm hai đồng xu nên tổng số thao tác cũng có thể được lấy trực tiếp. Tổng số xu cần thiết là`(y + z - 2x) + (z - x) + (y - x) = 2(y + z - 2x)`. 

Chia cho hai sẽ có đáp án`y + z - 2x`. 

Do đó, toàn bộ vấn đề quy về việc sắp xếp ba số và áp dụng một công thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời) | O(1) | Quá chậm, lên tới 999.999.998 lần lặp | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba giá trị rương và sắp xếp chúng sao cho`x <= y <= z`. Việc sắp xếp ba giá trị mất thời gian không đổi và cho phép chúng ta suy luận về rương nhỏ nhất, giữa và lớn nhất mà không cần xử lý các trường hợp riêng biệt cho vị trí ban đầu của chúng. 
2. Xét giá trị chung cuối cùng`T`. Nhu cầu ngực nhỏ nhất`T - x`thêm tiền, trong khi hai đồng còn lại cần`T - y`Và`T - z`. 
3. Yêu cầu mức thâm hụt lớn nhất không lớn hơn tổng của hai khoản thâm hụt còn lại. Điều này là cần thiết vì mỗi đồng xu được thêm vào rương nhỏ nhất phải kèm theo một đồng xu được thêm vào một trong các rương khác. Kể từ đây`T - x <= T - y + T - z`. 
4. Sắp xếp lại bất đẳng thức để có được`T >= y + z - x`. Việc chọn mục tiêu nhỏ nhất có thể sẽ mang lại`T = y + z - x`, giúp giảm thiểu khối lượng công việc. 
5. Tính mức tăng cần thiết của rương nhỏ nhất:`T - x = y + z - 2x`. 

Tại mục tiêu này, mức tăng này chính xác là tổng của hai mức tăng còn lại, do đó tồn tại một chuỗi các hoạt động cặp hợp lệ. 
6. Đầu ra`y + z - 2x`. Đây đã là số lượng thao tác vì tổng số đồng xu được thêm vào gấp đôi giá trị này và mỗi thao tác thêm chính xác hai đồng xu. 

Điều bất biến đằng sau việc xây dựng là mức tăng yêu cầu của rương nhỏ nhất bằng mức tăng yêu cầu tổng hợp của hai rương còn lại. Chúng ta có thể ghép mọi đồng xu cần thiết cho rương nhỏ nhất với chính xác một đồng xu bắt buộc cho một trong hai rương còn lại. Không có ca phẫu thuật nào cần phải chạm vào cùng một bộ ngực hai lần và cả ba điểm thiếu hụt đều được giải quyết đồng thời. Bất kỳ mục tiêu nhỏ hơn nào cũng sẽ vi phạm điều kiện ghép nối, vì vậy không có giải pháp nào có thể sử dụng ít thao tác hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = int(input())
b = int(input())
c = int(input())

x, y, z = sorted((a, b, c))

print(y + z - 2 * x)
```Ba dòng đầu tiên ghi ba kích cỡ ngực. Không có số lượng trường hợp kiểm thử trong đầu vào nên chương trình xử lý chính xác một trường hợp. 

Sắp xếp mang lại`x <= y <= z`, khớp với các biến được sử dụng trong đạo hàm. biểu hiện`y + z - 2 * x`là số lượng thao tác trực tiếp, do đó không cần phải tính toán rõ ràng mục tiêu cuối cùng hoặc mô phỏng các hoạt động riêng lẻ. 

Số nguyên Python có độ chính xác tùy ý, do đó phép nhân với`2`là an toàn ngay cả khi giá trị đầu vào lớn bằng`5 * 10^8`. Trong ngôn ngữ có số nguyên có chiều rộng cố định, biểu thức tương tự vẫn nằm trong phạm vi số nguyên có dấu 32 bit ở đây, nhưng việc sử dụng loại số nguyên rộng hơn cũng sẽ vô hại. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`1, 2, 3`. Sau khi sắp xếp, chúng ta có`x = 1`,`y = 2`, Và`z = 3`. 

| x | y | z | Mục tiêu`y + z - x`| Trả lời`y + z - 2x`| 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 4 | 3 | 

Giá trị chung cuối cùng là`4`. Ba rương cần`3`,`2`, Và`1`tiền bổ sung tương ứng. Những sự gia tăng đó có thể được kết hợp như`(1,3)`,`(1,2)`, Và`(1,2)`, đưa ra chính xác ba thao tác và tạo ra`4, 4, 4`. 

Đối với Mẫu 2, cả ba rương đều chứa hai đồng xu. 

| x | y | z | Mục tiêu`y + z - x`| Trả lời`y + z - 2x`| 
| --- | --- | --- | --- | --- | 
| 2 | 2 | 2 | 2 | 0 | 

Mức tăng yêu cầu của mỗi rương là bằng 0 nên không cần phẫu thuật. Công thức xử lý trường hợp này một cách tự nhiên mà không cần điều kiện đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có ba giá trị được sắp xếp và một biểu thức số học được đánh giá. | 
| Không gian | O(1) | Chỉ có ba biến số nguyên được lưu trữ. | 

Giá trị đầu vào có thể lớn bằng`5 * 10^8`, nhưng thuật toán không bao giờ lặp dựa trên độ lớn của chúng. Nó thực hiện một lượng công việc cố định, do đó giới hạn thời gian 1 giây và giới hạn bộ nhớ 512 MB có thể dễ dàng được đáp ứng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    a = int(input())
    b = int(input())
    c = int(input())

    x, y, z = sorted((a, b, c))
    print(y + z - 2 * x)

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
assert run("1\n2\n3\n") == "3\n", "sample 1"
assert run("2\n2\n2\n") == "0\n", "sample 2"

# Minimum-size input
assert run("1\n1\n1\n") == "0\n", "minimum values and already equal"

# Maximum-size input
assert run("1\n500000000\n500000000\n") == "999999998\n", "maximum values"

# Repeated maximum values
assert run("1\n3\n3\n") == "4\n", "repeated maximum values"

# Repeated minimum values
assert run("2\n2\n5\n") == "3\n", "repeated minimum values"

# Values in unsorted order
assert run("5\n1\n3\n") == "6\n", "input order must not matter"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`0`| Giá trị tối thiểu và các rương đã bằng nhau | 
|`1 500000000 500000000`|`999999998`| Câu trả lời tối đa và số học có giá trị lớn | 
|`1 3 3`|`4`| Giá trị tối đa lặp lại | 
|`2 2 5`|`3`| Giá trị tối thiểu lặp lại | 
|`5 1 3`|`6`| Việc sắp xếp là cần thiết vì thứ tự đầu vào là tùy ý | 

## Vỏ cạnh 

Đối với trường hợp đã bằng nhau```
2
2
2
```sắp xếp mang lại`x = y = z = 2`. Công thức trở thành`2 + 2 - 4 = 0`. Không có hoạt động nào được thực hiện, đó là kết quả tối thiểu có thể. 

Đối với trường hợp chiếc rương lớn nhất ban đầu có vẻ như có thể được giữ nguyên,```
1
2
3
```chúng tôi nhận được`x = 1`,`y = 2`,`z = 3`. Để lại rương lớn nhất ở`3`sẽ yêu cầu tăng`2, 1, 0`, không thể được tạo ra bằng các phép toán cặp. Thay vào đó, mục tiêu khả thi nhỏ nhất là`2 + 3 - 1 = 4`. Mức tăng cần thiết là`3, 2, 1`, và ba đơn vị cần thiết cho rương nhỏ nhất có thể được ghép nối với hai đơn vị cần thiết cho rương giữa và một đơn vị cần thiết cho rương lớn nhất. Công thức trả về`2 + 3 - 2 = 3`. 

Đối với các giá trị tối đa lặp lại,```
1
3
3
```các giá trị được sắp xếp là`1, 3, 3`. Mục tiêu là`3 + 3 - 1 = 5`, vậy mức thâm hụt là`4, 2, 2`. Bốn thao tác là đủ, ghép cặp rương đầu tiên với mỗi rương khác hai lần. Công thức cho`3 + 3 - 2 = 4`. 

Đối với trường hợp có kích thước tối đa,```
1
500000000
500000000
```câu trả lời là`500000000 + 500000000 - 2 = 999999998`. Mô phỏng từng giây sẽ cần gần một tỷ lần lặp, trong khi công thức sẽ tính kết quả ngay lập tức. Trường hợp này chứng tỏ tại sao việc dẫn xuất theo thời gian không đổi là cần thiết thay vì chỉ đơn thuần là một cách triển khai rõ ràng hơn. 

Chỉnh sửa 

## Hiểu vấn đề 

Chúng tôi có ba cái rương chứa`a`,`b`, Và`c`tiền xu. Trong một giây, chúng tôi chọn chính xác hai rương khác nhau và thêm một đồng xu vào mỗi rương đã chọn. Chúng tôi cần cả ba rương kết thúc với cùng một số xu và chúng tôi muốn có số giây tối thiểu. 

Đầu vào chứa ba số tiền xu ban đầu, mỗi số nằm giữa`1`Và`5 * 10^8`. Chỉ có ba giá trị liên quan nên không cần cấu trúc dữ liệu phức tạp. Giới hạn trên lớn là tín hiệu thực: một mô phỏng thực hiện một thao tác mỗi giây có thể yêu cầu gần một tỷ lần lặp, vượt xa giới hạn 1 giây cho phép. Một giải pháp thời gian không đổi là mục tiêu thích hợp. 

Trường hợp chính là khi rương lớn nhất đã có giá trị mà ban đầu chúng ta có thể coi là mục tiêu. Ví dụ, với`1, 2, 3`, chỉ cần nâng hai rương đầu tiên lên`3`không hoạt động vì mức tăng yêu cầu của chúng là khác nhau, trong khi rương thứ ba thì không cần tăng. Câu trả lời đúng là`3`, đạt`4, 4, 4`. 

Một trường hợp cạnh khác là một cấu hình đã bằng nhau, chẳng hạn như`2, 2, 2`. Câu trả lời là`0`, do đó mô phỏng thực hiện một thao tác trước khi kiểm tra đẳng thức sẽ không chính xác. 

Các giá trị lặp lại cũng quan trọng. Vì`1, 3, 3`, câu trả lời là`4`, không`2`. Mọi thao tác tăng rương đầu tiên cũng phải tăng 1 trong 2 rương còn lại nên mục tiêu phải cao hơn mức tối đa hiện tại. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng các hoạt động từng giây một. Ở mỗi bước, chúng ta có thể chọn một cặp rương giúp chúng ta hướng tới sự bình đẳng, cập nhật giá trị của chúng và dừng lại khi cả ba giá trị đều khớp. Điều này đúng với việc lựa chọn các cặp phù hợp, nhưng số giây có thể rất lớn. 

Sau khi sắp xếp các giá trị như`x <= y <= z`, trường hợp xấu nhất theo các ràng buộc là`x = 1`,`y = z = 5 * 10^8`. Vậy thì câu trả lời tối thiểu là`999,999,998`, do đó, một mô phỏng sẽ thực hiện gần một tỷ lần lặp. 

Quan sát chính là lý giải về mức tăng cần thiết thay vì các hoạt động riêng lẻ. Giả sử giá trị chung cuối cùng là`T`. Ba rương cần`T-x`,`T-y`, Và`T-z`tiền bổ sung. Vì mỗi thao tác sẽ thêm một đồng xu vào hai rương khác nhau nên mức tăng yêu cầu lớn nhất không thể vượt quá tổng của hai rương còn lại. 

Thâm hụt lớn nhất thuộc về rương nhỏ nhất, vì vậy`T - x <= (T - y) + (T - z)`. 

Sắp xếp lại mang lại`T >= y + z - x`. 

Do đó, mục tiêu khả thi nhỏ nhất là`T = y + z - x`. Tại mục tiêu đó, mức tăng cần thiết là`y + z - 2x`,`z - x`, Và`y - x`. Số lượng đầu tiên chính xác là tổng của hai số còn lại, vì vậy mọi đồng xu bắt buộc cho rương nhỏ nhất có thể được ghép với một xu bắt buộc cho một trong các rương khác. 

Do đó, tổng số tiền được thêm vào là gấp đôi`y + z - 2x`, và mọi thao tác đều cộng chính xác hai đồng xu. Số lượng hoạt động tối thiểu chỉ đơn giản là`y + z - 2x`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời) | O(1) | Quá chậm, lên tới 999.999.998 lần lặp | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba giá trị rương và sắp xếp chúng sao cho`x <= y <= z`. Việc sắp xếp ba số cần có thời gian không đổi và cho phép chúng ta suy luận về rương nhỏ nhất, giữa và lớn nhất. 
2. Giả sử giá trị chung cuối cùng là`T`. Ba mức tăng bắt buộc là`T-x`,`T-y`, Và`T-z`. 
3. Rương nhỏ nhất có mức thâm hụt lớn nhất. Mỗi đồng xu được thêm vào phải kèm theo một đồng xu được thêm vào một trong hai rương còn lại, do đó mức thâm hụt của nó không thể vượt quá tổng số tiền thâm hụt của chúng. 
4. Giải quyết`T-x <= T-y + T-z`, thu được`T >= y + z - x`. Do đó, mục tiêu nhỏ nhất có thể là`T = y + z - x`. 
5. Ở mục tiêu này, rương nhỏ nhất cần`y + z - 2x`tiền xu. Hai rương còn lại cần`z-x`Và`y-x`, tổng của nó chính xác là`y + z - 2x`. 
6. Vì mức thâm hụt lớn nhất bằng tổng của hai khoản còn lại nên tất cả các khoản tăng cần thiết đều có thể được kết hợp thành các hoạt động hợp lệ. Do đó, số lượng hoạt động`y + z - 2x`. 

Điều bất biến là mỗi hoạt động đóng góp một đơn vị vào đúng hai mức thâm hụt cần thiết. Tại mục tiêu đã chọn, mức thâm hụt lớn nhất chính xác bằng tổng quy mô của hai khoản thâm hụt còn lại, do đó mức tăng cần thiết có thể được ghép đôi hoàn toàn. Bất kỳ mục tiêu nhỏ hơn nào cũng sẽ khiến mức thâm hụt lớn nhất trở nên quá lớn để có thể ghép đôi, chứng tỏ rằng câu trả lời được tính toán là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = int(input())
b = int(input())
c = int(input())

x, y, z = sorted((a, b, c))

print(y + z - 2 * x)
```Đầu vào bao gồm chính xác ba dòng, do đó không có vòng lặp test-case. Sắp xếp đặt các giá trị theo thứ tự được sử dụng bởi đạo hàm toán học. 

Biểu thức cuối cùng trực tiếp tính toán số lượng thao tác, do đó việc triển khai không cần xây dựng trạng thái cuối cùng hoặc mô phỏng bất kỳ thao tác nào. Số nguyên Python cũng xử lý tất cả các giá trị trung gian một cách an toàn. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, các giá trị là`1, 2, 3`. 

| x | y | z | Mục tiêu | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 4 | 3 | 

Mức tăng cần thiết là`3, 2, 1`. Chúng có thể được ghép nối thành ba hoạt động, tạo ra`4, 4, 4`. 

Đối với Mẫu 2, các giá trị đã bằng nhau. 

| x | y | z | Mục tiêu | Trả lời | 
| --- | --- | --- | --- | --- | 
| 2 | 2 | 2 | 2 | 0 | 

Mọi mức thâm hụt đều bằng 0, vì vậy công thức ngay lập tức trả về 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ sắp xếp ba giá trị và đánh giá một công thức | 
| Không gian | O(1) | Chỉ một số nguyên không đổi được lưu trữ | 

Thuật toán không bao giờ thực hiện công việc tỷ lệ thuận với số lượng xu. Ngay cả khi câu trả lời là gần một tỷ, nó chỉ thực hiện một số phép tính số học cố định, do đó, nó vừa vặn thoải mái với giới hạn 1 giây và 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    a = int(input())
    b = int(input())
    c = int(input())

    x, y, z = sorted((a, b, c))
    print(y + z - 2 * x)

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

assert run("1\n2\n3\n") == "3\n", "sample 1"
assert run("2\n2\n2\n") == "0\n", "sample 2"

assert run("1\n1\n1\n") == "0\n", "minimum values"
assert run("1\n500000000\n500000000\n") == "999999998\n", "maximum values"
assert run("1\n3\n3\n") == "4\n", "repeated maximum values"
assert run("2\n2\n5\n") == "3\n", "repeated minimum values"
assert run("5\n1\n3\n") == "6\n", "unsorted input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`0`| Giá trị tối thiểu và đẳng thức | 
|`1 500000000 500000000`|`999999998`| Câu trả lời tối đa | 
|`1 3 3`|`4`| Giá trị tối đa lặp lại | 
|`2 2 5`|`3`| Giá trị tối thiểu lặp lại | 
|`5 1 3`|`6`| Độc lập với thứ tự đầu vào | 

## Vỏ cạnh 

cho`2, 2, 2`, sắp xếp cho`x = y = z = 2`, và câu trả lời là`2 + 2 - 4 = 0`. Thuật toán nhận dạng chính xác rằng không cần thực hiện thao tác nào. 

Vì`1, 2, 3`, mục tiêu khả thi nhỏ nhất là`2 + 3 - 1 = 4`. Những thâm hụt là`3, 2, 1`, như vậy rương nhỏ nhất có thể ghép với rương giữa hai lần và rương lớn nhất một lần. Kết quả là`3`. 

Vì`1, 3, 3`, mục tiêu là`3 + 3 - 1 = 5`. Những thâm hụt là`4, 2, 2`, cho phép bốn hoạt động hợp lệ. Công thức trả về`3 + 3 - 2 = 4`. 

Vì`1, 500000000, 500000000`, kết quả là`999999998`. Mô phỏng trực tiếp sẽ yêu cầu nhiều lần lặp như vậy, trong khi thuật toán tối ưu sẽ đạt được câu trả lời ngay lập tức thông qua biểu thức dạng đóng.
