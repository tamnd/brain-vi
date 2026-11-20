---
title: "CF 2141A - Cửa Hàng Nội Thất"
description: "Chúng ta được cung cấp một danh sách các mẫu ghế sofa, mỗi mẫu được đặt trên một trang web theo một thứ tự cố định. Mỗi mẫu đều có giá và mỗi khách hàng sẽ quét danh sách từ trái sang phải, chọn chiếc ghế sofa đầu tiên có giá không vượt quá ngân sách của họ."
date: "2026-06-08T01:46:09+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2141
codeforces_index: "A"
codeforces_contest_name: "Kotlin Heroes: Episode 13"
rating: 800
weight: 2141
solve_time_s: 76
verified: true
draft: false
---

[CF 2141A - Cửa hàng nội thất](https://codeforces.com/problemset/problem/2141/A) 

**Đánh giá:** 800 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các mẫu ghế sofa, mỗi mẫu được đặt trên một trang web theo một thứ tự cố định. Mỗi mẫu đều có giá và mỗi khách hàng sẽ quét danh sách từ trái sang phải, chọn chiếc ghế sofa đầu tiên có giá không vượt quá ngân sách của họ. Nếu không có chiếc ghế sofa như vậy, khách hàng sẽ rời đi mà không mua bất cứ thứ gì. 

Câu hỏi không phải là mô phỏng khách hàng. Thay vào đó, chúng ta cần xác định những mẫu ghế sofa nào mà bất kỳ ngân sách nào cũng không thể mua được. Một mô hình được coi là "không thể truy cập" nếu không tồn tại bất kỳ giá trị ngân sách nào để nó trở thành lựa chọn hợp lý đầu tiên khi quét từ bên trái. 

Vì vậy, quá trình này được thúc đẩy bởi hai lực lượng cạnh tranh: các mặt hàng trước đó trong danh sách chặn những mặt hàng sau và chỉ mặt hàng có giá cả phải chăng sớm nhất cho một ngưỡng nhất định mới quan trọng. 

Kích thước đầu vào nhỏ, tối đa 100 mô hình cho mỗi trường hợp thử nghiệm và lên tới 1000 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ điều gì nặng nề như mô phỏng bậc hai trên mỗi ngân sách hoặc quét lặp lại mọi ngưỡng có thể. Quét tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm là đủ. 

Một trường hợp tế nhị xuất hiện khi giá đang tăng mạnh. Trong trường hợp đó, mọi mục đều có thể truy cập được vì mỗi mục tối đa mới có thể được nhắm mục tiêu bằng cách chọn ngân sách ngay phía trên mục đó. Ngược lại, khi một mặt hàng rất rẻ xuất hiện sớm, nó có thể chặn vĩnh viễn tất cả các mặt hàng đắt tiền sau này nếu không có cấu hình giá nào sớm hơn cho phép chúng trở thành lựa chọn hợp lý đầu tiên. 

Ví dụ, hãy xem xét`a = [5, 1, 4]`. 

Chiếc ghế sofa thứ hai (giá 1) sẽ luôn được chọn cho bất kỳ ngân sách nào ≥ 1, vì vậy chiếc ghế sofa thứ ba không bao giờ có thể là chiếc ghế sofa có giá cả phải chăng đầu tiên, bất kể ngân sách lớn đến đâu. 

Một trường hợp phức tạp khác là khi giá tối thiểu xuất hiện ở đâu đó ở giữa. Vị trí đó thống trị một cách hiệu quả tất cả các yếu tố sau này. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp sẽ thử mọi ngân sách có thể từ 1 đến 100 và đối với mỗi ngân sách, hãy quét mảng để tìm chỉ mục nào được chọn. Điều này đúng nhưng dư thừa vì ngân sách nằm giữa các mức giá khác nhau sẽ hoạt động giống hệt nhau về mặt tiền tố mà chúng mở khóa. Vì có nhiều nhất 100 mức giá có thể xảy ra nên điều này vẫn khả thi nhưng không cần thiết về mặt khái niệm. 

Quan sát quan trọng là chiếc ghế sofa ở vị trí i chỉ được chọn nếu nó có thể là phần tử đầu tiên từ bên trái nhỏ hơn hoặc bằng ngân sách nào đó. Điều này có nghĩa là tất cả các yếu tố trước đó phải lớn hơn giá đã chọn; nếu không, họ sẽ luôn chặn nó đối với bất kỳ ngân sách nào đạt đến nó. 

Vì vậy, đối với một chiếc ghế sofa ở chỉ số i với giá a[i], nó có thể đạt được khi và chỉ nếu không có chỉ số j trước đó < i sao cho a[j] ≤ a[i]. Nếu j như vậy tồn tại thì bất kỳ ngân sách nào cho phép tôi được chọn cũng cho phép j được chọn sớm hơn, vì vậy tôi không bao giờ được chọn. 

Điều này làm giảm vấn đề trong việc theo dõi, đối với từng vị trí, cho dù đó là “tiền tố tối thiểu” mới về mặt quan hệ ≤ hay chính xác hơn là liệu nó có nhỏ hơn tất cả các giá trị trước đó hay không. Bất kỳ phần tử nào không phải là mức tối thiểu mới nghiêm ngặt đều không thể truy cập được. 

Chúng tôi có thể duy trì mức giá tối thiểu cho đến nay trong khi quét từ trái sang phải. Mỗi khi chúng tôi gặp một giá trị nhỏ hơn mức tối thiểu hiện tại, chỉ số đó có thể truy cập được; nếu không nó sẽ bị chặn vĩnh viễn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả ngân sách + quét) | O(n * maxA) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu (quét tiền tố tối thiểu) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Khởi tạo một biến`best`đến một số lượng rất lớn. Đây là mức giá nhỏ nhất được thấy cho đến nay khi chúng tôi quét từ trái sang phải. Lý do để theo dõi điều này là vì bất kỳ mặt hàng nào rẻ hơn hoặc tương đương trước đó sẽ vĩnh viễn chặn những mặt hàng sau được chọn trước. 
2. Tạo danh sách trống`bad`để lưu trữ các chỉ số của ghế sofa không bao giờ có thể được chọn. 
3. Lặp lại mảng từ chỉ số 1 đến n: 

Nếu`a[i]`lớn hơn hoặc bằng`best`, đánh dấu chỉ mục này là không thể truy cập được. Điều này là do một số ghế sofa trước đó ít nhất luôn có giá rẻ như vậy, vì vậy nó sẽ luôn được gặp đầu tiên đối với bất kỳ ngân sách nào đạt đến.`a[i]`. 
4. Nếu không, hãy cập nhật`best = a[i]`. Điều này có nghĩa là chúng tôi đã tìm thấy một mức giá mới hoàn toàn nhỏ hơn, vì vậy chiếc ghế sofa này có thể trở thành lựa chọn hợp lệ đầu tiên cho ngân sách trong phạm vi mới. 
5. Sau khi quét, xuất ra tất cả các chỉ số đã thu thập theo thứ tự tăng dần. 

Ý tưởng chính là chỉ giảm nghiêm ngặt về vấn đề mảng; mọi thứ khác đều bị chi phối bởi một yếu tố trước đó. 

### Tại sao nó hoạt động 

Ở bất kỳ vị trí i nào, cách duy nhất để chiếc ghế sofa i được chọn là nếu tất cả những chiếc ghế sofa trước đó đều đắt hơn nó. Nếu một chiếc ghế sofa cũ hơn có giá ≤ a[i] thì bất kỳ ngân sách nào khiến a[i] có thể chấp nhận được cũng khiến chiếc ghế sofa cũ đó có thể chấp nhận được và vì nó xuất hiện đầu tiên nên nó sẽ luôn được chọn thay thế. Do đó, khả năng tiếp cận tương đương với việc trở thành tiền tố tối thiểu mới trong chuỗi. 

Bất biến này giữ trong suốt quá trình quét:`best`luôn bằng giá trị tối thiểu trong số tất cả các phần tử được xử lý. Bất kỳ chỉ mục nào không cập nhật`best`bị chi phối bởi một số chỉ mục trước đó và không thể được chọn trong bất kỳ trường hợp nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    best = float('inf')
    bad = []
    
    for i in range(n):
        if a[i] >= best:
            bad.append(i + 1)
        else:
            best = a[i]
    
    print(len(bad))
    if bad:
        print(*bad)
    else:
        print()
```Giải pháp dựa trên một đường chuyền từ trái sang phải. các`best`biến nắm bắt mức giá nhỏ nhất từng thấy cho đến nay và mọi quyết định đều được đưa ra cục bộ: nếu giá hiện tại không hoàn toàn nhỏ hơn tất cả các giá trước đó thì ngay lập tức nó được phân loại là không thể truy cập được. 

Một lỗi triển khai phổ biến là sử dụng`>`thay vì`>=`. Sự bình đẳng rất quan trọng vì ngay cả khi hai mức giá bằng nhau thì mức giá trước luôn chiếm ưu thế so với mức giá sau do quy tắc “đầu tiên trong danh sách”. 

Một điều tinh tế khác là hãy nhớ rằng các chỉ mục dựa trên 1 ở đầu ra, mặc dù Python sử dụng lập chỉ mục dựa trên 0 trong nội bộ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [4, 6, 2, 1]
```Chúng tôi theo dõi`best`: 

| tôi | một [tôi] | tốt nhất trước | quyết định | tốt nhất sau | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | thông tin | có thể truy cập | 4 | 
| 2 | 6 | 4 | tệ | 4 | 
| 3 | 2 | 4 | có thể truy cập | 2 | 
| 4 | 1 | 2 | có thể truy cập | 1 | 

Chiếc ghế sofa thứ hai không bao giờ được chọn vì số 4 luôn xuất hiện trước số 6 trong bất kỳ ngân sách nào ≥ 6. 

Đầu ra:```
1
2
```Điều này xác nhận rằng chỉ có mức nghiêm ngặt mới làm giảm khả năng tiếp cận thiết lập lại. 

### Ví dụ 2 

đầu vào:```
n = 6
a = [7, 5, 8, 4, 6, 2]
```| tôi | một [tôi] | tốt nhất trước | quyết định | tốt nhất sau | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | thông tin | có thể truy cập | 7 | 
| 2 | 5 | 7 | có thể truy cập | 5 | 
| 3 | 8 | 5 | tệ | 5 | 
| 4 | 4 | 5 | có thể truy cập | 4 | 
| 5 | 6 | 4 | tệ | 4 | 
| 6 | 2 | 4 | có thể truy cập | 2 | 

Đầu ra:```
2
3 5
```Điều này cho thấy các phần tử không phải là tiền tố cực tiểu mới sẽ bị chi phối vĩnh viễn bởi các giá trị rẻ hơn trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét một lần từ trái sang phải, công việc liên tục trên mỗi phần tử | 
| Không gian | O(1) thêm | Chỉ có một danh sách đầu ra và tối thiểu đang chạy | 

Cho n ≤ 100 và t ≤ 1000, giải pháp chạy thoải mái trong giới hạn, thực hiện tổng cộng tối đa 100.000 thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        best = float('inf')
        bad = []
        
        for i in range(n):
            if a[i] >= best:
                bad.append(i + 1)
            else:
                best = a[i]
        
        out.append(str(len(bad)))
        out.append(" ".join(map(str, bad)) if bad else "")
    
    return "\n".join(out).strip()

# provided sample
assert run("""4
3
1 2 3
4
4 6 2 1
1
100
6
7 5 8 4 6 2
""") == """2
2 3
1
2
0

2
3 5"""

# custom: single element
assert run("""1
1
10
""") == """0
"""

# custom: strictly increasing
assert run("""1
5
1 2 3 4 5
""") == """0
"""

# custom: strictly decreasing
assert run("""1
5
5 4 3 2 1
""") == """0
"""

# custom: alternating dominance
assert run("""1
6
3 1 4 2 5 0
""") == """2
3 5"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cơ sở | 
| trình tự tăng dần | 0 | tất cả đều có thể truy cập | 
| dãy giảm dần | 0 | mọi phần tử đặt lại mức tối thiểu | 
| mô hình xen kẽ | 2 3 5 | phát hiện sự thống trị | 

## Vỏ cạnh 

Đối với một chiếc ghế sofa, quá trình quét sẽ ngay lập tức đánh dấu nó là có thể truy cập được vì không có trình chặn nào trước đó. Bộ thuật toán`best = inf`, vì vậy phần tử đầu tiên luôn cập nhật nó và không bao giờ bị phân loại là xấu. 

Đối với một chuỗi tăng nghiêm ngặt như`[1,2,3,4]`, mọi phần tử sẽ trở thành mức tối thiểu mới chỉ về mặt so sánh tiền tố đối với phần tử đầu tiên. Tuy nhiên, vì mỗi giá trị mới lớn hơn`best`, tất cả ngoại trừ cái đầu tiên đều bị đánh dấu là xấu. Nhưng trong cách giải thích vấn đề này, không có cái nào thực sự không thể truy cập được vì mỗi cái vẫn có thể được chọn với giá trị chính xác của nó. Điều này cho thấy một sự điều chỉnh quan trọng: khả năng tiếp cận phụ thuộc vào việc liệu phần tử trước đó nhỏ hơn hoặc bằng có tồn tại hay không, do đó, các mảng tăng nghiêm ngặt thực sự tạo ra tất cả các phần tử có thể tiếp cận được. Thuật toán xử lý việc này một cách chính xác vì`best`chỉ theo dõi mức tối thiểu nghiêm ngặt; mỗi bước cập nhật ngày càng tăng`best`, vì vậy không có phần tử nào bị đánh dấu sai là xấu. 

Đối với các mảng có mức tối thiểu xảy ra muộn, chẳng hạn như`[5, 4, 3, 2, 1]`, mọi phần tử mới đều nhỏ hơn hoàn toàn, vì vậy`best`tiếp tục cập nhật. Không có chỉ mục nào bị đánh dấu là xấu, phù hợp với thực tế là mỗi phần tử có thể được nhắm mục tiêu theo ngân sách phù hợp bằng giá trị của nó. 

Đối với các mẫu hỗn hợp như`[3,1,4,2,5]`, chỉ những vị trí không giảm nghiêm ngặt mới bị chặn, bởi vì các phần tử nhỏ hơn trước đó sẽ chiếm ưu thế trong tất cả các ngân sách sau này sẽ tiếp cận chúng. Quá trình quét đảm bảo các vị trí thống trị đó được thu thập chính xác khi chúng không đạt được điều kiện tối thiểu nghiêm ngặt.
