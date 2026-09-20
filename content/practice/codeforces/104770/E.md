---
title: "CF 104770E - Sự hỗn loạn kế toán"
description: "Chúng tôi được cung cấp một danh sách các khoản phí được ghi trong nhật ký khách sạn. Mỗi mục tương ứng với một số dịch vụ được cung cấp trong thời gian Sergey lưu trú, bao gồm cả chính căn phòng đó."
date: "2026-06-28T19:52:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104770
codeforces_index: "E"
codeforces_contest_name: "The XXXI Saint-Petersburg High School Programming Contest (SpbKOSHP 2023) | Qualification for the XXIV Russia Open High School Programming Contest (VKOSHP 2023)"
rating: 0
weight: 104770
solve_time_s: 82
verified: false
draft: false
---

[CF 104770E - Sự hỗn loạn trong kế toán](https://codeforces.com/problemset/problem/104770/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các khoản phí được ghi trong nhật ký khách sạn. Mỗi mục tương ứng với một số dịch vụ được cung cấp trong thời gian Sergey lưu trú, bao gồm cả chính căn phòng đó. Điều khiến dữ liệu trở nên lộn xộn là chúng ta không còn biết mục nào tương ứng với loại dịch vụ nào nữa, chỉ còn lại chi phí thô. 

Nguyên tắc cấu trúc quan trọng là mọi loại dịch vụ đều có giá cố định trong suốt thời gian lưu trú. Điều đó có nghĩa là nếu một dịch vụ xuất hiện nhiều lần trong nhật ký thì tất cả những lần xuất hiện đó đều có cùng giá trị nhưng chúng tôi không thể phân biệt chúng với các dịch vụ khác có cùng chi phí. Một dịch vụ đặc biệt là phòng và được biết rằng Sergey đã ở chính xác`n`ngày, vì vậy dịch vụ phòng phải được tính phí chính xác`n`tổng số lần. 

Nhiệm vụ là xác định tất cả các giá trị có thể đại diện cho chi phí phòng hàng ngày, nhất quán với ý tưởng rằng chúng ta có thể gán từng bản ghi cho một số loại dịch vụ và dịch vụ phòng phải tính toán chính xác.`n`của`m`hồ sơ. 

Kích thước đầu vào lớn, lên tới 300.000 bản ghi. Điều đó ngay lập tức loại trừ mọi suy luận bậc hai theo cặp hoặc quét lại toàn bộ mảng cho mỗi ứng cử viên. Bất kỳ giải pháp nào kiểm tra tính khả thi trên mỗi giá trị đều phải thực hiện theo thời gian tuyến tính hoặc gần tuyến tính nói chung. 

Một cạm bẫy tinh vi là giả định rằng chi phí phòng phải xuất hiện chính xác`n`lần. Điều đó là không bắt buộc; ít nhất nó chỉ cần xuất hiện`n`lần trong nhiều bộ để chúng ta có thể chọn`n`số lần xuất hiện trong số đó để đại diện cho căn phòng trong nhiều ngày. Các lần xuất hiện bổ sung có thể được hiểu đơn giản là các dịch vụ khác có cùng mức giá. 

Một sự nhầm lẫn tiềm ẩn khác nảy sinh từ việc các dịch vụ tư duy được nhóm lại mỗi ngày. Không có hạn chế mỗi ngày về số lượng dịch vụ được thực hiện ngoài thực tế là có`n`tổng số ngày và phòng xuất hiện một lần mỗi ngày. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi giá trị chi phí riêng biệt làm ứng cử viên cho giá phòng. Đối với mỗi giá trị ứng cử viên`x`, chúng ta đếm xem nó xuất hiện bao nhiêu lần trong danh sách và kiểm tra xem nó có xuất hiện ít nhất không`n`lần. Nếu vậy chúng ta có thể gán`n`của những lần xuất hiện đó đối với dịch vụ phòng và giải thích những lần xuất hiện còn lại là các dịch vụ khác cùng loại. Điều này đúng vì không có gì ngăn cản việc sử dụng một loại dịch vụ cho nhiều mục nhật ký riêng biệt. 

Ý tưởng bạo lực này là chính xác, nhưng việc quét mới tất cả`m`các mục nhập cho mỗi giá trị riêng biệt sẽ dẫn đến trường hợp xấu nhất là`O(m^2)`khi tất cả các giá trị đều khác biệt, quá chậm để`m = 3 × 10^5`. 

Điều quan trọng là chúng ta không cần phải quét nhiều lần. Tính khả thi của một giá trị chỉ phụ thuộc vào tần số của nó. Khi chúng tôi tính toán tần số cho tất cả các giá trị trong một lần, chúng tôi có thể lọc trực tiếp những tần số có tần số ít nhất`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên mỗi giá trị | O(m^2) | O(m) | Quá chậm | 
| Đếm tần số | O(m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, và danh sách các chi phí. Mục đích là để hiểu tần suất mỗi chi phí xuất hiện. 
2. Xây dựng bản đồ tần số của tất cả các giá trị trong danh sách. Điều này ghi lại số lần mỗi giá dịch vụ có thể xuất hiện trên tạp chí. 
3. Lặp lại tất cả các giá trị riêng biệt trong bản đồ tần số. 
4. Với mỗi giá trị`x`, kiểm tra xem tần số của nó có ít nhất là`n`. Nếu có, bao gồm`x`trong bộ câu trả lời. Lý do là chúng ta có thể gán`n`về những sự việc này xảy ra với dịch vụ phòng trên khắp`n`ngày. 
5. In ra số lượng ứng viên hợp lệ và danh sách theo thứ tự bất kỳ. 

### Tại sao nó hoạt động 

Mỗi giá trị chi phí riêng biệt tương ứng với một loại dịch vụ tiềm năng, vì tất cả các lần xuất hiện của cùng một giá trị có thể được coi là một dịch vụ có giá cố định. Ràng buộc duy nhất đối với một giá trị đại diện cho phòng là chúng ta phải có khả năng chỉ định chính xác một mục nhập phòng mỗi ngày trên`n`ngày, đòi hỏi ít nhất`n`lần xuất hiện. Không còn ràng buộc về cấu trúc nào nữa vì những lần xuất hiện không được sử dụng luôn có thể được hiểu là các bản ghi dịch vụ bổ sung cùng loại. Điều này làm cho tần số trở thành yếu tố quyết định duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    c = list(map(int, input().split()))
    
    freq = {}
    for x in c:
        freq[x] = freq.get(x, 0) + 1
    
    res = []
    for x, f in freq.items():
        if f >= n:
            res.append(x)
    
    print(len(res))
    print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai hoàn toàn dựa vào từ điển tần số được xây dựng trong một lần duy nhất. Điều này tránh việc quét lặp lại. Bước lọc cuối cùng là tuyến tính theo số lượng các giá trị riêng biệt, nhiều nhất là`m`. 

Một lỗi phổ biến là cố gắng mô phỏng các nhiệm vụ mỗi ngày hoặc xây dựng các nhóm rõ ràng. Điều đó là không cần thiết vì hạn chế toàn cầu duy nhất là tổng số lần xuất hiện chứ không phải cấu trúc mỗi ngày. 

## Ví dụ đã hoạt động 

### Dấu vết mẫu 

đầu vào:```
2 10
1 3 6 5 3 2 4 5 3 2
```Tần số: 

| Giá trị | Tần số | ≥ n (=2)? | 
| --- | --- | --- | 
| 1 | 1 | Không | 
| 2 | 2 | Có | 
| 3 | 3 | Có | 
| 4 | 1 | Không | 
| 5 | 2 | Có | 
| 6 | 1 | Không | 

Bộ câu trả lời trở thành`{2, 3, 5}`. 

Điều này cho thấy nhiều loại dịch vụ không liên quan đều có thể là giá phòng hợp lệ miễn là chúng xuất hiện đủ thường xuyên. 

### Ví dụ về phân phối biên 

đầu vào:```
3 6
7 7 7 8 8 9
```Tần số: 

| Giá trị | Tần số | ≥ n (=3)? | 
| --- | --- | --- | 
| 7 | 3 | Có | 
| 8 | 2 | Không | 
| 9 | 1 | Không | 

Chỉ một`7`hoạt động, vì chỉ có nó mới có thể bao gồm cả ba ngày. 

Điều này chứng tỏ rằng sự bằng nhau về tần số với`n`là đủ và cấu trúc bổ sung là không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Một lượt để xây dựng tần số và một lượt vượt qua các giá trị riêng biệt | 
| Không gian | O(m) | Lưu trữ bản đồ tần số trong trường hợp xấu nhất khi tất cả các giá trị khác nhau | 

Lời giải dễ dàng nằm trong giới hạn vì`3 × 10^5`các hoạt động không đáng kể trong Python để đếm bản đồ băm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    def solve():
        n, m = map(int, input().split())
        c = list(map(int, input().split()))
        freq = {}
        for x in c:
            freq[x] = freq.get(x, 0) + 1
        res = [x for x, f in freq.items() if f >= n]
        print(len(res))
        print(*res)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided sample
assert run("2 10\n1 3 6 5 3 2 4 5 3 2\n") == "3\n2 3 5"

# minimum case
assert run("1 1\n100\n") == "1\n100"

# all equal
assert run("3 5\n7 7 7 7 7\n") == "1\n7"

# no duplicates but still valid n=1
assert run("1 4\n1 2 3 4\n") == "4\n1 2 3 4"

# boundary heavy frequency
assert run("5 10\n9 9 9 9 9 1 2 3 4 5\n") == "1\n9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn tối thiểu | 1 giá trị | độ đúng cơ sở | 
| đều có tần số lớn bằng nhau | hợp lệ duy nhất | hành vi ngưỡng tần số | 
| tất cả đều khác biệt n=1 | tất cả đều hợp lệ | trường hợp ràng buộc yếu nhất | 
| phân phối lệch | một hợp lệ | lọc tính đúng đắn | 

## Vỏ cạnh 

Khi nào`n = 1`, mọi giá trị đều hợp lệ vì bất kỳ sự xuất hiện nào cũng có thể được coi là tiền phòng cho ngày hôm đó. Thuật toán xử lý việc này một cách tự nhiên vì mỗi tần số có ít nhất một tần số, do đó tất cả các khóa đều được bao gồm. 

Khi tất cả các giá trị giống hệt nhau thì tần số bằng`m`và thuật toán trả về chính xác giá trị đó nếu`m ≥ n`. Việc giải thích nhóm vẫn đúng vì chúng ta có thể gán chính xác`n`sự cố xảy ra trong phòng. 

Khi các giá trị hầu hết là duy nhất chỉ có một lần lặp lại đủ số lần thì chỉ giá trị lặp lại đó mới vượt qua ngưỡng tần số. Việc đếm dựa trên từ điển đảm bảo chúng tôi không coi nhầm các lần xuất hiện đơn lẻ là ứng cử viên hợp lệ.
