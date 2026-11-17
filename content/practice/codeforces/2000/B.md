---
title: "CF 2000B - Chỗ ngồi trên xe buýt"
description: "Chúng ta có một chiếc xe buýt có n chỗ ngồi xếp thành một hàng, được đánh số từ 1 đến n. Hành khách lần lượt lên xe và mỗi người chọn một chỗ ngồi theo một quy tắc đơn giản: hành khách đầu tiên có thể ngồi ở bất cứ đâu, nhưng hành khách tiếp theo phải ngồi cạnh một ghế đã có người ngồi."
date: "2026-06-09T02:30:43+07:00"
tags: ["codeforces", "competitive-programming", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2000
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 966 (Div. 3)"
rating: 800
weight: 2000
solve_time_s: 204
verified: false
draft: false
---

[CF 2000B - Chỗ ngồi trên xe buýt](https://codeforces.com/problemset/problem/2000/B) 

**Đánh giá:** 800 
**Thẻ:** hai con trỏ 
**Thời gian giải:** 3 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chiếc xe buýt với`n`ghế xếp thành một hàng, được đánh số từ 1 đến`n`. Hành khách lần lượt lên xe và mỗi người chọn một chỗ ngồi theo một quy tắc đơn giản: hành khách đầu tiên có thể ngồi ở bất cứ đâu, nhưng hành khách tiếp theo phải ngồi cạnh một ghế đã có người ngồi. Chúng ta được sắp xếp theo thứ tự thời gian các chỗ ngồi được chọn bởi`n`hành khách và phải xác định xem mỗi hành khách có tuân theo khuyến nghị này hay không. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp`n`và dãy số ghế. Đầu ra là "CÓ" nếu mọi hành khách trong trường hợp thử nghiệm đó tuân theo quy tắc liền kề hoặc "KHÔNG" nếu ngược lại. 

Những ràng buộc cho phép`n`để đạt 2·10^5 cho mỗi trường hợp thử nghiệm, với tối đa 10^4 trường hợp thử nghiệm, mặc dù tổng tổng của tất cả`n`trên tất cả các trường hợp thử nghiệm tối đa là 2·10^5. Điều này ngụ ý rằng chúng ta không thể chấp nhận được cách tiếp cận O(n^2), vì nó sẽ thực hiện khoảng 4·10^10 thao tác trong trường hợp xấu nhất. Chúng ta cần một giải pháp có độ phức tạp tuyến tính tổng thể hoặc gần tuyến tính. 

Trường hợp khó khăn xảy ra khi một số hành khách đầu tiên chiếm giữ hai đầu xe buýt. Ví dụ, nếu`n = 5`và trình tự là`[5, 4, 2, 1, 3]`, hành khách thứ ba chọn chỗ ngồi`2`, điều này không hợp lệ vì không có hàng xóm nào bị chiếm đóng. Một cách tiếp cận ngây thơ chỉ kiểm tra xem số ghế có tồn tại trong chuỗi hay không cho đến nay sẽ bỏ sót điều này vì các ghế đã sử dụng không liền kề nhau tại thời điểm đó. 

Một trường hợp tinh vi khác xảy ra khi hành khách chiếm chỗ ngồi theo thứ tự tăng hoặc giảm nghiêm ngặt từ một đầu, điều này hợp lệ vì mọi chỗ ngồi mới đều liền kề với phân khúc đã có người hiện tại. Việc phát hiện phân đoạn liên tục của số ghế có người ngồi là rất quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi hành khách sau hành khách đầu tiên, hãy kiểm tra ngay các ghế ở bên trái và bên phải và xác minh xem một trong hai ghế có bị ngồi hay không. Điều này hoạt động hợp lý, nhưng mỗi lần kiểm tra sẽ yêu cầu quét danh sách chỗ ngồi đã có người, dẫn đến O(n^2) cho mỗi trường hợp kiểm thử. Điều này trở nên không khả thi đối với quy mô lớn`n`. 

Quan sát quan trọng là tại bất kỳ thời điểm nào, tập hợp các ghế hợp lệ sẽ tạo thành một khối liền kề các ghế đã được sử dụng. Mỗi hành khách mới phải mở rộng khu vực này bằng cách ngồi ngay cạnh ghế ngồi ngoài cùng bên trái hoặc ngoài cùng bên phải của mình. Vì vậy, chúng ta chỉ cần theo dõi số ghế ngồi ngoài cùng bên trái và ngoài cùng bên phải hiện tại. Đối với hành khách đầu tiên, chúng tôi đặt cả hai đầu vào chỗ ngồi đó. Mỗi hành khách tiếp theo phải ngồi ngay bên trái của đầu bên trái hiện tại hoặc ngay bên phải của đầu bên phải hiện tại. Nếu hành khách chọn chỗ ngồi ngoài hai lựa chọn này thì sẽ vi phạm quy định. 

Quan sát này cho phép quét O(n) của chuỗi trong khi duy trì hai biến,`left`Và`right`, đại diện cho phân khúc được sử dụng hiện tại. Không cần cấu trúc dữ liệu phức tạp. Giải pháp này đơn giản và hiệu quả, lý tưởng cho các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét tất cả các ghế đã có người) | O(n^2) | O(n) | Quá chậm | 
| Theo dõi phân đoạn (hai con trỏ) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai con trỏ,`left`Và`right`, ĐẾN`-1`. Chúng sẽ theo dõi khối chỗ ngồi liền kề hiện tại đã được sử dụng. 
2. Lặp lại trình tự lựa chọn chỗ ngồi của hành khách. 
3. Đối với hành khách đầu tiên, đặt cả hai`left`Và`right`đến số ghế của họ. Điều này thiết lập phân khúc ban đầu. 
4. Đối với mỗi hành khách tiếp theo, hãy kiểm tra xem ghế của họ có ở ngay bên trái không (`seat == left - 1`) hoặc ngay bên phải (`seat == right + 1`) của phân đoạn hiện tại. 
5. Nếu ghế mở rộng về phía bên trái, hãy cập nhật`left`tới chỗ ngồi mới này. Nếu nó mở rộng về phía bên phải, hãy cập nhật`right`. Nếu nó không mở rộng cả hai đầu, trả về "NO" cho trường hợp kiểm thử này. 
6. Nếu tất cả hành khách thỏa mãn điều kiện liền kề, trả về “CÓ”. 

Tại sao tính năng này hoạt động: bằng cách duy trì các ghế được sử dụng ngoài cùng bên trái và ngoài cùng bên phải, chúng tôi đảm bảo rằng các ghế được sử dụng sẽ tạo thành một phân đoạn liền kề. Vì hành khách chỉ được phép ngồi liền kề với đoạn này nên bất kỳ hành vi vi phạm nào ngay lập tức sẽ vi phạm quy định. Bất biến này được duy trì sau mỗi bước, đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def seating_in_bus():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        left = right = -1
        valid = True
        
        for seat in a:
            if left == -1:
                left = right = seat
            elif seat == left - 1:
                left = seat
            elif seat == right + 1:
                right = seat
            else:
                valid = False
                break
        
        print("YES" if valid else "NO")

if __name__ == "__main__":
    seating_in_bus()
```Mã khởi tạo`left`Và`right`ĐẾN`-1`để chỉ ra một chiếc xe buýt trống. Đối với mỗi hành khách, chúng tôi kiểm tra xem họ có ngồi cạnh phân khúc đang có người sử dụng hay không. Việc cập nhật ranh giới phân đoạn đảm bảo rằng các lần kiểm tra tiếp theo vẫn hợp lệ. Séc`seat == left - 1`Và`seat == right + 1`xử lý cả hai đầu một cách hiệu quả. Phá vỡ sớm ngay lần vi phạm đầu tiên sẽ tránh được những tính toán không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[5, 4, 2, 1, 3]`với`n = 5`| Hành khách | Chỗ ngồi | trái | đúng | Có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | 5 | CÓ | 
| 2 | 4 | 4 | 5 | CÓ | 
| 3 | 2 | 4 | 5 | KHÔNG | 

Hành khách 3 đã chọn chỗ ngồi`2`, không liền kề với phân khúc hiện tại`[4,5]`. Thuật toán xác định chính xác vi phạm này. 

### Ví dụ 2 

đầu vào:`[2, 3, 1]`với`n = 3`| Hành khách | Chỗ ngồi | trái | đúng | Có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | CÓ | 
| 2 | 3 | 2 | 3 | CÓ | 
| 3 | 1 | 1 | 3 | CÓ | 

Mọi hành khách đều mở rộng đoạn tiếp giáp, tạo ra "CÓ". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) trên mỗi trường hợp thử nghiệm, tổng thể O(Σn) | Mỗi chỗ ngồi được kiểm tra đúng một lần, việc cập nhật trái/phải mất O(1) | 
| Không gian | O(1) | Chỉ có hai con trỏ và biến vòng lặp được lưu trữ | 

Với Σn 2·10^5, thuật toán thực hiện thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    seating_in_bus()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("4\n5\n5 4 2 1 3\n3\n2 3 1\n4\n2 3 1 4\n5\n1 2 3 5 4\n") == "NO\nYES\nYES\nNO"

# Custom cases
assert run("1\n1\n1\n") == "YES", "single passenger"
assert run("1\n3\n3 2 1\n") == "YES", "decreasing sequence from end"
assert run("1\n4\n1 3 2 4\n") == "NO", "non-adjacent seat in middle"
assert run("1\n2\n2 1\n") == "YES", "two passengers occupying ends"
assert run("1\n5\n3 4 5 2 1\n") == "YES", "segment extends in both directions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`| CÓ | Trường hợp cạnh hành khách đơn | 
|`1\n3\n3 2 1`| CÓ | Giảm trình tự từ một đầu | 
|`1\n4\n1 3 2 4`| KHÔNG | Ghế không liền kề ở giữa | 
|`1\n2\n2 1`| CÓ | Hai hành khách ở cuối | 
|`1\n5\n3 4 5 2 1`| CÓ | Phân khúc phát triển chính xác theo cả hai hướng | 

## Vỏ cạnh 

Nếu hành khách đầu tiên ngồi ở giữa, ví dụ:`n = 5`, sự liên tiếp`[3, 2, 4, 1, 5]`, thuật toán khởi tạo`left=right=3`. Hành khách 2 ngồi ở`2`(`left-1`), đang cập nhật`left=2`. Hành khách 3 ngồi ở`4`(`right+1`), đang cập nhật`right=4`. Hành khách 4 ngồi ở`1`(`left-1`), đang cập nhật`left=1`. Hành khách 5 ngồi ở`5`(`right+1`), đang cập nhật`right=5`. Mỗi bước duy trì sự bất biến của một phân đoạn liền kề. Thuật toán xuất ra "CÓ" chính xác. Điều này khẳng định việc xử lý đúng đắn sự tăng trưởng của phân khúc không cần thiết theo cả hai hướng.
