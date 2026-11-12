---
title: "CF 1675A - Thức ăn cho động vật"
description: "Chúng ta được đưa vào một tình huống trong một cửa hàng thú cưng, nơi có ba loại thức ăn: thức ăn cho chó, thức ăn cho mèo và thức ăn phổ thông có thể cho chó hoặc mèo ăn. Polycarp sở hữu một số lượng chó và mèo nhất định. Nhiệm vụ của chúng ta là quyết định xem cửa hàng có đủ thức ăn để đáp ứng tất cả thú cưng của mình hay không."
date: "2026-06-10T01:04:26+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1675
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 787 (Div. 3)"
rating: 800
weight: 1675
solve_time_s: 97
verified: true
draft: false
---

[CF 1675A - Thức ăn cho động vật](https://codeforces.com/problemset/problem/1675/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa vào một tình huống trong một cửa hàng thú cưng, nơi có ba loại thức ăn: thức ăn cho chó, thức ăn cho mèo và thức ăn phổ thông có thể cho chó hoặc mèo ăn. Polycarp sở hữu một số lượng chó và mèo nhất định. Nhiệm vụ của chúng ta là quyết định xem cửa hàng có đủ thức ăn để đáp ứng tất cả thú cưng của mình hay không. Mỗi loài động vật cần chính xác một gói thức ăn phù hợp với loài của nó. 

Đầu vào cung cấp cho chúng tôi nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi biết số lượng gói chó, gói mèo, gói phổ thông và số lượng chó và mèo mà Polycarp sở hữu. Chúng ta phải xuất CÓ nếu anh ta có thể cho tất cả vật nuôi của mình ăn và KHÔNG nếu ngược lại. 

Những ràng buộc rất hào phóng. Mỗi giá trị có thể lên tới 10^8 và số lượng trường hợp thử nghiệm có thể lên tới 10^4. Điều này loại trừ mọi giải pháp lặp đi lặp lại cố gắng chỉ định các gói riêng lẻ cho từng thú cưng. Chúng ta cần một giải pháp hoạt động bằng phép so sánh số học trực tiếp thay vì mô phỏng. 

Các trường hợp đặc biệt phát sinh khi số lượng động vật khớp chính xác với số lượng gói thực phẩm cụ thể, khi chỉ có sẵn loại thực phẩm phổ biến hoặc khi một loài vượt quá số lượng thực phẩm chuyên dụng của nó. Ví dụ: nếu Polycarp có 100 con chó nhưng không có thức ăn cho chó và 50 gói phổ thông thì kết quả đầu ra chính xác là KHÔNG, vì ngay cả với tất cả các gói phổ thông, 50 con chó vẫn chưa được cho ăn. Một giải pháp bất cẩn có thể cho rằng bất kỳ loại lương thực phổ thông nào cũng đủ mà không so sánh nó với tình trạng thiếu hụt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng việc đưa cho mỗi con chó và mèo một gói thức ăn có sẵn, giảm số lượng khi chúng ta tiếp tục. Điều này sẽ hoạt động về mặt khái niệm nhưng không hiệu quả vì chúng ta có thể có tới 10^8 con vật hoặc gói thức ăn và việc lặp đi lặp lại nhiều lần như vậy là không thể trong thực tế. 

Thông tin chi tiết quan trọng là chúng ta chỉ cần kiểm tra xem tổng số gói cụ thể và phổ biến có bao gồm từng loài một cách độc lập hay không. Đầu tiên, hãy kiểm tra xem số lượng đàn chó có ít nhất bằng số lượng chó hay không. Nếu không, sự khác biệt phải được bù đắp bằng thực phẩm phổ thông. Sau đó kiểm tra mèo tương tự, sử dụng các gói phổ thông còn lại sau khi phục vụ chó. Nếu ở bất kỳ bước nào các gói phổ quát còn lại đều âm thì câu trả lời là KHÔNG; nếu không thì CÓ. Cách tiếp cận này làm giảm vấn đề thành các phép so sánh và phép trừ số học đơn giản, có giá trị O(1) cho mỗi trường hợp thử nghiệm, phù hợp thoải mái trong các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(x + y) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm đối với đầu vào lớn | 
| So Sánh Số Học | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test, trích xuất số lượng gói thức ăn cho chó`a`, gói thức ăn cho mèo`b`, gói thực phẩm phổ thông`c`, và số lượng chó`x`và mèo`y`. 
3. Tính lượng thức ăn thiếu hụt cho chó như sau`max(0, x - a)`. Điều này thể hiện số lượng chó không thể được cho ăn bằng thức ăn dành riêng cho chó và phải dựa vào thức ăn phổ thông. 
4. Tính lượng thức ăn thiếu hụt cho mèo tương tự như`max(0, y - b)`. 
5. Kiểm tra xem tổng số phần thiếu hụt này có vượt quá số lượng gói phổ thông không`c`. Nếu như`deficit_dogs + deficit_cats > c`, đầu ra KHÔNG; nếu không thì xuất ra CÓ. 
6. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Thuật toán đảm bảo rằng tất cả chó và mèo trước tiên đều được cố gắng cho ăn loại thức ăn tương ứng của chúng. Chỉ những nhu cầu còn sót lại mới được tính vào lương thực phổ thông, và nếu lương thực phổ thông không đủ cho những thức ăn thừa này thì việc cho tất cả động vật ăn là không thể. Không có vấn đề đặt hàng tiềm ẩn nào vì thực phẩm phổ thông có thể thay thế cho nhau và chúng tôi chỉ quan tâm đến tổng mức thâm hụt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c, x, y = map(int, input().split())
    deficit_dogs = max(0, x - a)
    deficit_cats = max(0, y - b)
    if deficit_dogs + deficit_cats <= c:
        print("YES")
    else:
        print("NO")
```Giải pháp này đọc tất cả các trường hợp thử nghiệm một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`để xử lý các đầu vào lớn. Đối với mỗi trường hợp, nó tính toán chính xác số lượng động vật phải dựa vào nguồn thức ăn phổ biến. sử dụng`max(0, x - a)`đảm bảo rằng chúng tôi không tính thâm hụt âm nếu thực phẩm cụ thể là đủ. Sự so sánh với`c`trực tiếp xác định tính khả thi. Thứ tự của các phép tính rất quan trọng: chúng ta phải kiểm tra chó trước, nhưng về mặt toán học thì điều đó không quan trọng vì chúng ta chỉ quan tâm đến tổng số phần thiếu hụt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`1 1 4 2 3`| Biến | Giá trị | 
| --- | --- | 
| một | 1 | 
| b | 1 | 
| c | 4 | 
| x | 2 | 
| y | 3 | 
| thâm hụt_dogs | tối đa(0, 2-1) = 1 | 
| thâm hụt_cats | tối đa(0, 3-1) = 2 | 
| tổng số thâm hụt | 1 + 2 = 3 | 
| c | 4 | 

Vì 3 <= 4 nên đầu ra là CÓ. Thức ăn phổ thông bao gồm 1 con chó và 2 con mèo còn lại. 

### Ví dụ 2 

đầu vào:`0 0 0 0 0`Tất cả số lượng đều bằng 0, tất cả động vật đều bằng 0. Thâm hụt là 0, tổng là 0, thực phẩm phổ thông là 0. Đầu ra CÓ. 

Những dấu vết này cho thấy thuật toán xác định chính xác khi nào đủ thực phẩm phổ thông và khi nào thì không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp kiểm thử được xử lý bằng một vài phép tính số học và t có thể lên tới 10^4 | 
| Không gian | O(1) | Chúng tôi chỉ lưu trữ các biến thiếu hụt cho mỗi trường hợp thử nghiệm; không có cấu trúc dữ liệu lớn | 

Với những ràng buộc này, giải pháp chạy thoải mái dưới 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        a, b, c, x, y = map(int, input().split())
        deficit_dogs = max(0, x - a)
        deficit_cats = max(0, y - b)
        output.append("YES" if deficit_dogs + deficit_cats <= c else "NO")
    return "\n".join(output)

# Provided samples
assert run("7\n1 1 4 2 3\n0 0 0 0 0\n5 5 0 4 6\n1 1 1 1 1\n50000000 50000000 100000000 100000000 100000000\n0 0 0 100000000 100000000\n1 3 2 2 5\n") == "YES\nYES\nNO\nYES\nYES\nNO\nNO", "sample 1"

# Custom cases
assert run("1\n0 0 0 1 0\n") == "NO", "one dog, no food"
assert run("1\n0 0 1 1 0\n") == "YES", "one dog, one universal pack"
assert run("1\n10 10 5 15 15\n") == "NO", "more animals than total food"
assert run("1\n0 0 0 0 0\n") == "YES", "no animals, no food"
assert run("1\n100000000 100000000 100000000 100000000 100000000\n") == "YES", "max input values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 1 0 | KHÔNG | Xử lý thức ăn không đủ cho một con vật | 
| 0 0 1 1 0 | CÓ | Thực phẩm phổ thông dùng để bù đắp thâm hụt | 
| 10 10 5 15 15 | KHÔNG | Tổng thâm hụt vượt quá lương thực phổ thông | 
| 0 0 0 0 0 | CÓ | Không có động vật, hài lòng tầm thường | 
| 100000000 100000000 100000000 100000000 100000000 | CÓ | Xử lý giá trị tối đa | 

## Vỏ cạnh 

Để giá trị đầu vào tối đa với mỗi loại thực phẩm bằng số lượng con vật, thuật toán sẽ tính toán chính xác`deficit_dogs = 0`Và`deficit_cats = 0`, vì vậy đầu ra là CÓ. Nếu thực phẩm cụ thể bằng 0 nhưng thực phẩm phổ thông chính xác bằng tổng số động vật, thì thuật toán sẽ phân bổ chính xác mức thâm hụt cho thực phẩm phổ thông và sản lượng CÓ. Những thâm hụt tiêu cực được tránh bằng cách`max(0, x - a)`tính toán, tránh sai sót khi thừa thực phẩm cụ thể. Điều này đảm bảo tính chính xác trên các điều kiện biên.
