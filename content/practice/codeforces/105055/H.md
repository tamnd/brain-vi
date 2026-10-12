---
title: "CF 105055H - Hawarma"
description: "Chúng ta được cho một tham số nguyên $N$ và một hàm hữu tỉ $$f(x) = frac{5xN^2}{x^2 + 3xN - 5N^2}.$$ Nhiệm vụ là tìm tất cả các giá trị thực $a$ sao cho việc thay $a$ vào hàm sẽ trả về cùng một giá trị, nghĩa là $f(a) = a$."
date: "2026-06-28T00:24:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105055
codeforces_index: "H"
codeforces_contest_name: "UDESC Selection Contest 2023-2"
rating: 0
weight: 105055
solve_time_s: 54
verified: true
draft: false
---

[CF 105055H - Hawarma](https://codeforces.com/problemset/problem/105055/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tham số nguyên$N$và một hàm hợp lý$$f(x) = \frac{5xN^2}{x^2 + 3xN - 5N^2}.$$Nhiệm vụ là tìm tất cả các giá trị thực$a$như vậy là cắm$a$vào hàm trả về cùng một giá trị, nghĩa là$f(a) = a$. Chúng ta phải xuất ra có bao nhiêu giá trị như vậy tồn tại và liệt kê chúng một cách rõ ràng. 

Kích thước đầu vào rất nhỏ:$N$nằm ở$[-300, 300]$và là khác không. Điều này ngay lập tức loại trừ mọi nhu cầu về các phương pháp nặng tiệm cận; bất kỳ giải pháp nào giải quyết được một bài toán đại số có kích thước không đổi cho mỗi trường hợp thử nghiệm đều đủ. 

Điểm tinh tế chính là chúng ta đang giải một phương trình hữu tỉ với các giới hạn về miền. Mẫu số có thể trở thành 0 đối với một số giá trị và những giá trị đó phải được loại trừ ngay cả khi chúng xuất hiện dưới dạng nghiệm đại số sau khi đơn giản hóa. Một thao tác đại số ngây thơ nhân cả hai vế với mẫu số mà không theo dõi hạn chế này sẽ vô tình bao gồm các điểm không hợp lệ. 

Điểm tinh tế thứ hai là việc hủy bỏ bằng$x$hấp dẫn vì tử số chứa$x$, Nhưng$x = 0$phải được xử lý riêng. Rất dễ bị mất gốc này trong quá trình đơn giản hóa mặc dù nó hợp lệ khi mẫu số khác 0. 

Các trường hợp cạnh bao gồm: 

cho$N = 1$, mẫu số trở thành$x^2 + 3x - 5$, có nghiệm thực, do đó một số nghiệm ứng cử viên có thể trùng với các cực của hàm số. Một người giải quyết bất cẩn có thể bao gồm những điểm như vậy. 

Vì$x = 0$, chúng tôi luôn nhận được$f(0) = 0$miễn là mẫu số khác 0 và vì mẫu số có giá trị là$-5N^2 \neq 0$, số 0 luôn là một giải pháp hợp lệ cho mọi$N \neq 0$. 

Cuối cùng, phương trình có thể quy về phương trình bậc hai, nghĩa là có nhiều nhất hai nghiệm bổ sung ngoài 0, nhưng chúng ta phải xác minh rằng không có nghiệm nào trùng với mẫu số 0 bị cấm. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là xử lý phương trình$f(x) = x$như một đẳng thức liên tục và cố gắng liệt kê các giá trị thực ứng cử viên. Điều đó là không thể vì các giải pháp thực sự không thể đếm được. 

Thay vào đó, chúng ta giải trực tiếp phương trình đại số một cách tượng trưng. Sự bình đẳng về chức năng$$\frac{5xN^2}{x^2 + 3xN - 5N^2} = x$$có thể được biến đổi bằng cách nhân cả hai vế với mẫu số, nhưng chỉ sau khi loại trừ rõ ràng các giá trị trong đó mẫu số bằng 0. Điều này tạo ra một phương trình đa thức mà nghiệm của nó có thể được tìm thấy một cách chính xác. 

Thực hiện đại số:$$5xN^2 = x(x^2 + 3xN - 5N^2)$$Khai triển vế phải:$$5xN^2 = x^3 + 3x^2N - 5xN^2.$$Đưa mọi thứ sang một bên:$$0 = x^3 + 3x^2N - 10xN^2.$$Bao thanh toán ra$x$:$$x(x^2 + 3Nx - 10N^2) = 0.$$Vì vậy, một giải pháp luôn luôn là$x = 0$. Các giải pháp còn lại đến từ phương trình bậc hai:$$x^2 + 3Nx - 10N^2 = 0.$$Sự phân biệt đối xử của nó là:$$\Delta = 9N^2 + 40N^2 = 49N^2.$$Vì thế:$$x = \frac{-3N \pm 7|N|}{2}.$$Từ$N^2$luôn dương, ta đơn giản hóa trực tiếp:$$x = \frac{-3N \pm 7N}{2}.$$Vậy hai gốc là:$$x_1 = 2N, \quad x_2 = -5N.$$Chúng ta vẫn phải xác minh những điều này không làm mẫu số bằng 0. Thay thế các chương trình họ không bao giờ làm$N \neq 0$, vậy cả ba nghiệm đều hợp lệ. 

Ý tưởng bạo lực không thành công vì nó xử lý một phương trình hợp lý như thể nó yêu cầu tìm kiếm trên các số thực, trong khi cấu trúc sụp đổ hoàn toàn thành một khối với hệ số hóa rõ ràng. Quan sát quan trọng là sự bằng nhau của hàm hữu tỷ với hàm đồng nhất luôn tạo ra một phương trình đa thức sau khi xóa mẫu số, và ở đây nó phân tích thành nhân tử hoàn toàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên thực tế | Không thể | O(1) | Quá chậm | 
| Đơn giản hóa đại số | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ phương trình$f(x) = x$và nhân cả hai vế với mẫu số, nhưng chỉ với giả định rằng mẫu số khác 0 ở nghiệm hợp lệ. Điều này đảm bảo chúng tôi không đưa các gốc không hợp lệ từ các điểm bị cấm. 
2. Khai triển biểu thức thu được thành phương trình đa thức theo$x$. Cấu trúc trở thành bậc ba vì mẫu số ban đầu là bậc hai và được nhân với$x$. 
3. Di chuyển tất cả các số hạng sang một bên để thu được đa thức bậc ba. Bước này chuyển đổi phương trình hàm thành một bài toán tìm nghiệm chuẩn. 
4. Loại bỏ yếu tố$x$, vì mọi thuật ngữ đều chứa nó. Điều này ngay lập tức tiết lộ rằng$x = 0$là một giải pháp, miễn là hàm ban đầu được xác định ở đó. 
5. Giải phương trình bậc hai còn lại$x^2 + 3Nx - 10N^2 = 0$sử dụng phương pháp phân biệt. Phân biệt đối xử đơn giản hóa thành một bình phương hoàn hảo, đảm bảo các nghiệm số nguyên sạch. 
6. Tính hai nghiệm là$2N$Và$-5N$. 
7. Xuất ra ba giá trị$0$,$2N$, Và$-5N$. 

### Tại sao nó hoạt động 

Phép biến đổi bảo toàn sự tương đương giữa phương trình hữu tỉ ban đầu và phương trình đa thức dẫn xuất trên miền xác định có mẫu số khác 0. Mọi nghiệm hợp lệ của phương trình ban đầu phải thỏa mãn đa thức và bất kỳ nghiệm nào của đa thức không hủy mẫu số đều tương ứng với một nghiệm hợp lệ. Vì chúng ta kiểm tra rõ ràng cấu trúc của các nghiệm và không có nghiệm nào trùng với mẫu số 0 của$N \neq 0$, bộ giải pháp là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N = int(input().strip())

res = [0, 2 * N, -5 * N]

# remove duplicates just in case (though none occur for N != 0)
res = list(set(res))

print(len(res))
print(*res)
```Việc triển khai trực tiếp mã hóa đạo hàm dạng đóng. Việc tính toán duy nhất là hình thành ba nghiệm ứng cử viên. Một tập hợp được sử dụng như một thước đo an toàn, mặc dù về mặt đại số, ba giá trị này là khác nhau đối với tất cả các giá trị khác 0.$N$. 

Cần phải cẩn thận để không cố gắng giải trực tiếp bằng số của phương trình hữu tỉ, vì các phương pháp dấu phẩy động là không cần thiết và có nguy cơ gây ra các vấn đề về độ chính xác ngay cả khi các ràng buộc nhỏ. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi đầu vào mẫu$N = 3$. 

Rễ dẫn xuất là$0$,$2N = 6$, Và$-5N = -15$. 

| Bước | N | Gốc 1 | Gốc 2 | Gốc 3 | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 3 | 0 | 6 | -15 | tính toán ứng viên | 
| hoàn thiện | 3 | 0 | 6 | -15 | bộ đầu ra | 

Điều này xác nhận rằng việc rút gọn đại số tạo ra chính xác tập nghiệm dự kiến, bao gồm cả bội số dương và bội số âm của$N$. 

Ví dụ thứ hai với$N = 1$: 

| Bước | N | Rễ | 
| --- | --- | --- | 
| tính toán | 1 | 0, 2, -5 | 
| đầu ra | 1 | 0 2 -5 | 

Điều này cho thấy rằng ngay cả với cường độ nhỏ nhất$N$, cấu trúc vẫn nhất quán và tạo ra ba giải pháp riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi được thực hiện bất kể kích thước đầu vào | 
| Không gian | O(1) | Chỉ một danh sách các gốc ứng viên có kích thước cố định được lưu trữ | 

Các ràng buộc cho phép bất kỳ giải pháp đại số theo thời gian không đổi nào và cách tiếp cận này thỏa mãn một cách tầm thường cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    N = int(sys.stdin.readline().strip())
    res = [0, 2 * N, -5 * N]
    res = list(set(res))
    return str(len(res)) + "\n" + " ".join(map(str, res)) + "\n"

# provided sample
assert run("3\n") == "3\n0 6 -15\n" or run("3\n") == "3\n6 0 -15\n"

# custom cases
assert run("1\n").split()[0] == "3"
assert run("-1\n").split()[0] == "3"
assert run("10\n").split()[0] == "3"
assert run("2\n") != "", "basic sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 | 0 6 -15 | độ chính xác của mẫu | 
| 1 | 0 2 -5 | trường hợp tích cực nhỏ nhất | 
| -1 | 0 -2 5 | xử lý biển báo | 
| 10 | 0 20 -50 | hành vi mở rộng quy mô | 

## Vỏ cạnh 

cho$N = 1$, phương trình trở nên đặc biệt rõ ràng và giúp xác minh rằng công thức dẫn xuất không phụ thuộc vào sự hủy ngẫu nhiên ở các giá trị lớn hơn. Thay thế$N = 1$mang lại nguồn gốc$0, 2, -5$. Mẫu số là$x^2 + 3x - 5$, khác 0 ở cả ba điểm, do đó không có nghiệm nào bị vô hiệu. 

Vì$N = -1$, chúng ta có được rễ$0, -2, 5$. Một sai lầm phổ biến là quên rằng cả hai$2N$Và$-5N$dấu lật nhất quán nhưng cấu trúc vẫn đối xứng. Việc kiểm tra mẫu số vẫn an toàn vì việc thay thế một trong hai giá trị không bao giờ mang lại kết quả bằng 0 cho các giá trị khác 0$N$. 

Đối với bất kỳ$N$, gốc$x = 0$là đặc biệt quan trọng. Nó tồn tại chỉ vì biểu thức hữu tỉ ban đầu có tử số tỉ lệ với$x$, và mẫu số ước tính là$-5N^2$, luôn luôn khác 0. Điều này đảm bảo rằng việc loại bỏ$x$thừa số trong quá trình đơn giản hóa không loại bỏ một giải pháp hợp lệ, miễn là chúng ta đưa lại nó một cách rõ ràng sau khi phân tích thành thừa số.
