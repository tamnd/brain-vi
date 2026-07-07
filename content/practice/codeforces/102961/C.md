---
title: "CF 102961C - Vòng đu quay"
description: "Chúng tôi được sắp xếp một hàng người, mỗi người có một trọng lượng và một vòng đu quay trong đó mỗi cabin có thể chứa tối đa hai người miễn là tổng trọng lượng của họ không vượt quá một giới hạn cố định."
date: "2026-07-04T06:50:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102961
codeforces_index: "C"
codeforces_contest_name: "CSES Problem Set: Sorting and Searching"
rating: 0
weight: 102961
solve_time_s: 44
verified: true
draft: false
---

[CF 102961C - Vòng đu quay](https://codeforces.com/problemset/problem/102961/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được sắp xếp một hàng người, mỗi người có một trọng lượng và một vòng đu quay trong đó mỗi cabin có thể chứa tối đa hai người miễn là tổng trọng lượng của họ không vượt quá một giới hạn cố định. Mỗi người phải được xếp vào đúng một cabin và mục tiêu là giảm thiểu số lượng cabin được sử dụng. 

Đầu vào mô tả số lượng người, giới hạn trọng lượng cho mỗi cabin và danh sách trọng lượng riêng lẻ. Đầu ra là một số duy nhất: số lượng cabin nhỏ nhất cần thiết để chứa tất cả mọi người với ràng buộc là mỗi cabin chứa một hoặc hai người có tổng trọng lượng không vượt quá giới hạn. 

Hạn chế chính thúc đẩy giải pháp là số lượng người có thể lớn, thường lên tới 200.000. Điều đó ngay lập tức loại trừ bất kỳ chiến lược ghép đôi bậc hai nào mà chúng tôi thử tất cả các kết hợp của mọi người. Một giải pháp kiểm tra từng cặp sẽ yêu cầu theo thứ tự hoạt động n2, sẽ không hoàn thành trong thời gian giới hạn. Ngay cả giải pháp O(n log n) cũng phải được thiết kế cẩn thận để việc sắp xếp không bị chi phối bởi quá trình quét lặp lại. 

Một trường hợp thất bại phổ biến xuất hiện khi áp dụng chiến lược tham lam mà không đặt hàng. Ví dụ: ghép nối từng người với người có sẵn tiếp theo theo thứ tự đầu vào có thể thất bại nặng nề. 

Hãy xem xét một đầu vào như:```
n = 4, x = 10
weights = [9, 1, 8, 2]
```Nếu chúng ta ghép các phần tử liền kề một cách tham lam, chúng ta sẽ nhận được (9,1) và (8,2), sử dụng 2 cabin và ở đây là tối ưu. Nhưng theo thứ tự hơi khác một chút:```
weights = [9, 8, 1, 2]
```Ghép nối liền kề cho (9,8) không hợp lệ, do đó, một sửa chữa đơn giản có thể cô lập 9, sau đó ghép 8 với 1 và để nguyên 2, dẫn đến 3 cabin. Phương án tối ưu là (9,1) và (8,2) chỉ có 2 cabin. Điều này cho thấy rằng nếu không có sự phân loại hoặc chiến lược toàn cầu, các quyết định cục bộ có thể cản trở các cặp đôi tốt hơn. 

Một vấn đề tế nhị khác là quên rằng việc ghép nối là tùy chọn. Ngay cả khi hai vật nặng nhỏ vừa khít với nhau, việc buộc chúng ghép đôi có thể ngăn cản việc ghép đôi vật nặng lớn hơn sau này, làm tăng tổng số cabin. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử phân công mọi người vào cabin theo mọi cách có thể, đặt từng người một mình hoặc ghép họ với một người khác có trọng lượng tổng hợp hợp lệ. Điều này tương ứng với việc khám phá tất cả các kết quả khớp trong biểu đồ tương thích trong đó tồn tại một cạnh nếu hai trọng số có tổng tối đa là giới hạn. Mặc dù đúng về nguyên tắc nhưng số lượng trạng thái tăng theo cấp số nhân vì mỗi người có thể được ghép đôi hoặc không ghép đôi trong nhiều cấu hình và các lựa chọn tương tác trên toàn cầu. Trong trường hợp xấu nhất, điều này trở thành vụ nổ tổ hợp theo thứ tự khoảng 2ⁿ trạng thái. 

Quan sát cấu trúc quan trọng là khi chúng ta sắp xếp các trọng số, chúng ta sẽ đạt được thứ tự đơn điệu cho phép chúng ta đưa ra các quyết định tham lam an toàn. Người nặng nhất còn lại là người khó đặt nhất. Nếu người đó có thể ghép đôi với bất kỳ ai thì ứng cử viên sáng giá nhất là người nhẹ cân nhất còn lại. Nếu ngay cả cái đó cũng không vừa thì người nặng nhất phải đi một mình. Điều này loại bỏ nhu cầu khám phá nhiều cặp đôi vì bất kỳ cặp đôi nào khác dành cho cặp nặng nhất sẽ chỉ sử dụng một đối tác nặng hơn và do đó không bao giờ tốt hơn. 

Điều này làm giảm vấn đề thành quy trình hai con trỏ trên một mảng được sắp xếp, trong đó chúng tôi liên tục cố gắng khớp các phần tử nhỏ nhất và lớn nhất còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tham lam tối ưu (Sắp xếp + Hai con trỏ) | O(n log n) | O(1) bổ sung / Tổng O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chiến lược tối ưu 

1. Sắp xếp tất cả các trọng số theo thứ tự không giảm. Điều này tạo ra một cấu trúc trong đó các phần tử nhẹ nhất và nặng nhất còn lại luôn dễ dàng được xác định. 
2. Khởi tạo hai con trỏ, một ở đầu mảng và một ở cuối mảng. Chúng đại diện cho những người nhẹ nhất và nặng nhất chưa được chỉ định. 
3. Duy trì một bộ đếm số lượng cabin được sử dụng. 
4. Trong khi con trỏ bên trái không vượt qua con trỏ bên phải, hãy xem xét người nặng nhất hiện tại ở con trỏ bên phải. 
5. Nếu vật nhẹ nhất và nặng nhất cùng nhau không vượt quá giới hạn, hãy chỉ định chúng vào cùng một cabin và di chuyển cả hai con trỏ vào trong. Điều này là hợp lý vì việc ghép cặp nặng nhất với đối tác nhỏ nhất có thể sẽ tối đa hóa cơ hội ghép đôi trong tương lai. 
6. Nếu vượt quá giới hạn, chỉ định người nặng nhất một mình và chỉ di chuyển con trỏ bên phải vào trong. Điều này là cần thiết vì không tồn tại cặp đôi hợp lệ nào cho người nặng nhất này theo ràng buộc hiện tại. 
7. Tăng bộ đếm cabin cho mỗi nhiệm vụ. 
8. Tiếp tục cho đến khi tất cả mọi người được phân công. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến rằng tất cả những người bên ngoài con trỏ hiện tại đã được chỉ định vào các cabin một cách tối ưu theo cùng một quy tắc tham lam. Ở mỗi bước, quyết định tập trung vào người nặng nhất còn lại. Bất kỳ việc ghép đôi khả thi nào cho người đó đều phải có sự tham gia của một người không nặng hơn ứng cử viên nhẹ nhất hiện tại, vì vậy việc kiểm tra trọng lượng nhỏ nhất còn lại là đủ để xác định xem có thể ghép đôi hay không. Vì việc chọn đối tác nhỏ nhất có thể không bao giờ làm xấu đi tính khả thi trong tương lai của các yếu tố còn lại, nên lựa chọn tham lam sẽ duy trì tính tối ưu trong suốt quá trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort()
    
    l, r = 0, n - 1
    ans = 0
    
    while l <= r:
        ans += 1
        if l == r:
            break
        if a[l] + a[r] <= x:
            l += 1
            r -= 1
        else:
            r -= 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng để chúng ta có thể suy luận về các điểm cực trị. Vòng lặp hai con trỏ luôn chỉ định chính xác một cabin cho mỗi lần lặp, cho một cặp hoặc một cá thể nặng. Trường hợp đặc biệt`l == r`xử lý người cuối cùng còn lại một cách sạch sẽ, ngăn ngừa việc đếm hai lần hoặc các nỗ lực ghép nối không hợp lệ. 

Một cạm bẫy triển khai phổ biến là quên tăng câu trả lời trong cả trường hợp ghép đôi và không ghép đôi. Một cách khác là xử lý sai điều kiện kết thúc, đặc biệt khi chỉ còn lại một phần tử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4, x = 10
weights = [9, 1, 8, 2]
```Sau khi sắp xếp:```
[1, 2, 8, 9]
```| Bước | Trái | Đúng | Thử theo cặp | Hành động | Cabin | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 9 | 1 + 9 = 10 | Cặp | 1 | 
| 2 | 2 | 8 | 2 + 8 = 10 | Cặp | 2 | 

Quá trình kết thúc với tất cả những người được phân công vào hai cabin. Điều này cho thấy cách sắp xếp biến một vấn đề ghép đôi dường như không có cấu trúc thành một quy trình ranh giới rõ ràng. 

### Ví dụ 2 

đầu vào:```
n = 5, x = 6
weights = [3, 5, 2, 1, 4]
```Đã sắp xếp:```
[1, 2, 3, 4, 5]
```| Bước | Trái | Đúng | Thử theo cặp | Hành động | Cabin | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 1 + 5 = 6 | Cặp | 1 | 
| 2 | 2 | 4 | 2 + 4 = 6 | Cặp | 2 | 
| 3 | 3 | 3 | độc thân | Một Mình | 3 | 

Dấu vết này làm nổi bật trường hợp có độ dài lẻ trong đó cuối cùng vẫn có một người không được ghép đôi, buộc phải sử dụng cabin dành cho một người. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét hai con trỏ là tuyến tính | 
| Không gian | O(1) thêm | Chỉ các con trỏ và bộ đếm được sử dụng ngoài bộ nhớ đầu vào | 

Giải pháp dễ dàng phù hợp với các ràng buộc đối với đầu vào lớn vì việc sắp xếp 200.000 phần tử nằm trong giới hạn thông thường và khả năng quét tuyến tính là không đáng kể so với nó. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from types import SimpleNamespace

    # re-run solution inline
    input = sys.stdin.readline

    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    l, r = 0, n - 1
    ans = 0

    while l <= r:
        ans += 1
        if l == r:
            break
        if a[l] + a[r] <= x:
            l += 1
            r -= 1
        else:
            r -= 1

    return str(ans)

# provided sample-style tests
assert run("4 10\n9 1 8 2\n") == "2"

# all fit individually
assert run("3 5\n4 4 4\n") == "3"

# all pairable
assert run("4 10\n1 2 3 4\n") == "2"

# alternating tight case
assert run("5 6\n3 5 2 1 4\n") == "3"

# single element
assert run("1 100\n42\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 10 / 9 1 8 2 | 2 | ghép đôi tham lam cơ bản | 
| 3 5 / 4 4 4 | 3 | không thể ghép nối | 
| 4 10 / 1 2 3 4 | 2 | có thể ghép nối đầy đủ | 
| 5 6 / 3 5 2 1 4 | 3 | ghép đôi + còn sót lại | 
| 1 100/42 | 1 | trường hợp cạnh phần tử đơn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chỉ có một người tồn tại. Sau khi sắp xếp, cả hai con trỏ đều bắt đầu ở cùng một chỉ mục. Thuật toán ngay lập tức tăng số lượng cabin một lần và thoát khi`l == r`, trả về chính xác 1. 

Một trường hợp khác là khi tất cả trọng số vượt quá một nửa giới hạn, khiến cho việc ghép nối không thể thực hiện được. Thuật toán luôn thất bại trong việc kiểm tra tổng và liên tục giảm con trỏ bên phải, chỉ gán cho mỗi người một người. Kết quả chính xác là n cabin, điều này đúng vì không tồn tại cặp hợp lệ nào. 

Trường hợp cuối cùng là khi tất cả các trọng số đều rất nhỏ, cho phép ghép nối đầy đủ. Thuật toán luôn thành công trong việc ghép nối các con trỏ trái và phải cho đến khi chúng gặp nhau hoặc giao nhau, tạo ra chính xác các cabin ⌈n/2⌉.
