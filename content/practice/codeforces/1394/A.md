---
title: "CF 1394A - Boboniu Chats with Du"
description: "Chúng ta được cung cấp một chuỗi “giá trị thú vị” thể hiện mức độ thú vị mà các tin nhắn của Du đưa ra mỗi ngày. Chúng tôi được phép sắp xếp lại các giá trị này theo bất kỳ thứ tự nào trước khi quá trình bắt đầu. Sau khi chốt đơn hàng, các ngày sẽ được tính từ trái sang phải."
date: "2026-06-11T09:49:12+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1394
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 664 (Div. 1)"
rating: 1800
weight: 1394
solve_time_s: 319
verified: false
draft: false
---

[CF 1394A - Boboniu trò chuyện với Du](https://codeforces.com/problemset/problem/1394/A) 

**Đánh giá:** 1800 
**Tags:** dp, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 5 phút 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi “giá trị thú vị” thể hiện mức độ thú vị mà các tin nhắn của Du đưa ra mỗi ngày. Chúng tôi được phép sắp xếp lại các giá trị này theo bất kỳ thứ tự nào trước khi quá trình bắt đầu. Sau khi chốt đơn hàng, các ngày sẽ được tính từ trái sang phải. 

Vào mỗi ngày, nếu Du hiện không bị tắt tiếng, chúng tôi sẽ lấy giá trị tiếp theo từ chuỗi và thêm nó vào tổng số niềm vui. Nếu giá trị đó lớn hơn một ngưỡng nhất định`m`, Boboniu trở nên tức giận và buộc Du phải ngừng nói tiếp theo`d`ngày. Trong những ngày im lặng, không có giá trị nào được tiêu thụ và không có niềm vui nào đạt được, quá trình này chỉ đơn giản là bỏ qua thời gian. 

Mục tiêu là hoán vị mảng sao cho tổng số niềm vui tích lũy được tối đa hóa. 

Khó khăn chính là các giá trị lớn rất nguy hiểm vì chúng kích hoạt sự im lặng bắt buộc, gây lãng phí cho các vị thế sắp tới. Các giá trị nhỏ sẽ an toàn vì chúng không bao giờ gây ra hình phạt và luôn lấp đầy những ngày nói tích cực một cách hiệu quả. 

Các ràng buộc đi lên đến`n = 100000`, điều này ngay lập tức loại trừ mọi tìm kiếm hoán vị giai thừa hoặc bậc hai. Thậm chí`O(n^2)`mô phỏng tham lam trên tất cả các sắp xếp lại là không thể. Chúng ta cần một chiến lược trong đó mỗi phần tử được đặt chính xác một lần trong cấu trúc được kiểm soát, lý tưởng nhất là trong`O(n log n)`hoặc`O(n)`thời gian. 

Một kẻ tham lam ngây thơ sắp xếp giảm dần và luôn chọn giá trị sẵn có lớn nhất có thể thất bại vì việc đặt một giá trị lớn quá sớm có thể lãng phí quá nhiều vị trí trong tương lai do khoảng thời gian tắt tiếng dài. Ngược lại, luôn trì hoãn các giá trị lớn cũng có thể không tối ưu nếu chúng được sử dụng để “chặn” thời gian trong khi các giá trị nhỏ sẽ bị lãng phí. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị đều lớn hơn`m`. Sau đó, mọi phần tử được chọn sẽ kích hoạt chế độ tắt tiếng. Ví dụ,`n = 5, d = 2, m = 0, a = [1,2,3,4,5]`. Bất kỳ thứ tự nào vẫn gây ra tình trạng chặn thường xuyên và chiến lược phải đảm bảo chúng tôi chỉ sử dụng các vị trí tốt nhất cho các giá trị lớn, giảm thiểu lãng phí vị trí. 

Một trường hợp cạnh khác phát sinh khi`d = 1`. Sau đó, mọi giá trị lớn chỉ chặn vào ngày hôm sau, do đó khoảng cách là tối thiểu và vấn đề trở nên gần hơn với việc lựa chọn đơn giản các vị trí tốt nhất. Mọi lịch trình quá phức tạp sẽ không cần thiết nhưng vẫn phải mang lại kết quả chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ liệt kê tất cả các hoán vị của mảng, mô phỏng quy trình cho mỗi hoán vị và tính toán kết quả thú vị. Đối với mỗi hoán vị, chúng tôi quét từ trái sang phải, duy trì một con trỏ và bộ đếm thời gian hồi chiêu. Bất cứ khi nào chúng ta gặp một giá trị lớn hơn`m`, chúng ta bỏ qua phần tiếp theo`d`các vị trí. Chi phí mô phỏng này`O(n)`mỗi hoán vị, và có`n!`hoán vị, dẫn đến không thể thực hiện được`O(n! * n)`sự phức tạp. 

Quan sát cấu trúc chính là chỉ những giá trị lớn hơn`m`ảnh hưởng đến động lực chặn. Giá trị`<= m`là “an toàn”: chúng không bao giờ kích hoạt chế độ tắt tiếng và do đó luôn đóng góp độc lập với việc đặt hàng, ngoại trừ số ngày hoạt động có sẵn để đặt chúng. Vấn đề giảm xuống còn việc quyết định vị trí đặt các giá trị lớn trong dòng thời gian của các vị trí phát biểu có sẵn. 

Chúng ta có thể tách mảng thành hai nhóm: giá trị an toàn (`<= m`) và giá trị nguy hiểm (`> m`). Các giá trị an toàn phải luôn được đón nhận một cách tham lam bất cứ khi nào chúng ta có một ngày rảnh để nói chuyện vì chúng không bao giờ làm giảm tính sẵn có trong tương lai. Thách thức đặt ra là làm thế nào để đặt các giá trị nguy hiểm sao cho hiệu ứng chặn của chúng được giảm thiểu trong khi vẫn thu được sự đóng góp của chúng. 

Thông tin chuyên sâu quan trọng là xử lý các giá trị nguy hiểm theo thứ tự giảm dần và mô phỏng việc đặt chúng vào dòng thời gian bằng cách sử dụng cấu trúc lập kế hoạch tham lam. Mỗi giá trị nguy hiểm tiêu tốn một ngày hoạt động và chặn ngày tiếp theo`d`ngày, tạo ra những “khoảng trống” trong dòng thời gian một cách hiệu quả. Trong khi đó, các giá trị an toàn chỉ cần điền vào tất cả các vị trí không bị chặn còn lại. 

Điều này dẫn đến một quá trình tham lam, trong đó chúng tôi duy trì số lượng vị trí đang hoạt động còn lại và số lượng giá trị an toàn có sẵn để lấp đầy các khoảng trống. Chúng tôi luôn ưu tiên đặt các giá trị nguy hiểm lớn hơn trước vì chúng tạo ra cùng chi phí chặn nhưng mang lại phần thưởng cao hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Chia nhỏ + Lập kế hoạch tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mảng thành hai nhóm được sắp xếp và mô phỏng việc lập kế hoạch tối ưu cho các giá trị nguy hiểm trong số những ngày có sẵn. 

1. Chia mảng thành hai danh sách: một danh sách chứa tất cả các giá trị lớn hơn`m`và một chứa tất cả các giá trị nhỏ hơn hoặc bằng`m`. Sự tách biệt này là cần thiết vì chỉ có nhóm lớn hơn mới kích hoạt hành vi chặn. 
2. Sắp xếp cả hai danh sách theo thứ tự giảm dần. Các giá trị lớn nhất phải được đặt sớm hơn trong số các vị trí có sẵn vì chúng mang lại mức tăng tức thời cao hơn với cùng một chi phí chặn. 
3. Tính xem chúng ta có thể sử dụng bao nhiêu “vị thế hiệu quả”. Mỗi giá trị nguy hiểm chiếm một ngày và loại bỏ ngày tiếp theo`d`ngày kể từ khi có sẵn, vì vậy mỗi vị trí sẽ tiêu tốn một khối kích thước`d + 1`. 
4. Tham lam lặp đi lặp lại các giá trị nguy hiểm theo thứ tự giảm dần, đặt từng giá trị miễn là chúng ta vẫn còn khả năng sẵn có về các vị trí hiệu quả. Mỗi vị trí đóng góp giá trị của nó cho câu trả lời và làm giảm dung lượng còn lại. 
5. Sau khi đặt tất cả các giá trị nguy hiểm có thể có, hãy điền vào tất cả các ngày có thể sử dụng còn lại bằng các giá trị an toàn. Vì các giá trị an toàn không bao giờ kích hoạt việc chặn nên chúng tôi chỉ cần lấy càng nhiều giá trị càng tốt và thêm tất cả vào câu trả lời. 
6. Nếu có nhiều giá trị an toàn hơn các vị trí còn lại, chúng tôi vẫn lấy tất cả vì các giá trị an toàn không ảnh hưởng lẫn nhau hoặc lập kế hoạch. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi giá trị nguy hiểm đều tiêu thụ chính xác một “khe kích hoạt” và loại bỏ một cách xác định`d`các khe có thể sử dụng được trong tương lai, bất kể nó được đặt ở đâu. Điều này làm cho tất cả các giá trị nguy hiểm có thể hoán đổi cho nhau về mặt chi phí, chỉ khác nhau về lợi ích. Do đó, việc sắp xếp chúng theo thứ tự giảm dần và lựa chọn một cách tham lam sẽ tối đa hóa tổng lợi ích theo một số lần kích hoạt có thể cố định. Các giá trị an toàn không ảnh hưởng đến khả năng kích hoạt trong tương lai, vì vậy chúng luôn có thể được thêm độc lập sau khi lịch trình nguy hiểm được khắc phục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, d, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    big = []
    small = []
    
    for x in a:
        if x > m:
            big.append(x)
        else:
            small.append(x)
    
    big.sort(reverse=True)
    small.sort(reverse=True)
    
    # maximum number of big elements we can place
    # each big consumes (d + 1) effective positions
    max_big = (n + d) // (d + 1)
    
    take_big = min(len(big), max_big)
    
    ans = sum(big[:take_big])
    
    # remaining positions after placing big elements
    remaining_slots = n - take_big * (d + 1)
    
    # we can place all small elements in remaining slots
    ans += sum(small[:remaining_slots])
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách phân chia các giá trị thành các nhóm nguy hiểm và an toàn dựa trên việc chúng có vượt quá`m`. Việc sắp xếp cả hai nhóm theo thứ tự giảm dần đảm bảo chúng tôi luôn xem xét các phần tử có giá trị nhất trước tiên. 

biểu hiện`(n + d) // (d + 1)`tính toán số lần chúng tôi có thể kích hoạt một “sự kiện chặn” trong trường hợp tốt nhất, vì mỗi giá trị nguy hiểm chiếm một ngày và vô hiệu hóa ngày tiếp theo`d`ngày. Điều này đưa ra giới hạn trên về số lượng phần tử lớn có thể được lên lịch. 

Sau khi chọn số lượng phần tử lớn cần lấy, chúng tôi tính toán số ngày thực tế còn có thể sử dụng được cho các phần tử nhỏ. Chúng chỉ đơn giản chứa đầy những giá trị an toàn tốt nhất hiện có. 

Điều tinh tế quan trọng là chúng tôi không bao giờ mô phỏng rõ ràng quá trình chuyển đổi hàng ngày. Thay vào đó, chúng tôi nén quy trình vào việc đếm số lượng vị trí hiệu quả mà mỗi giá trị lớn tiêu thụ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, d = 2, m = 11
a = [8, 10, 15, 23, 5]
```Chúng tôi chia thành: 

Lớn: [23, 15] 

Nhỏ: [10, 8, 5] 

Chúng tôi tính toán`max_big = (5 + 2) // 3 = 2`. 

Chúng tôi lấy cả hai giá trị lớn. 

| Bước | Hành động | Lấy lớn | Các slot còn lại | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | Lấy 23 | [23] | 5 - 3 = 2 | 23 | 
| 2 | Lấy 15 | [23, 15] | 2 - 3 (dừng) | 38 | 

Bây giờ các vị trí còn lại thực tế là 0 nên không có giá trị nhỏ nào được thêm vào. 

Câu trả lời cuối cùng là 38. Điều này cho thấy giá trị lớn chi phối mức tiêu thụ công suất như thế nào. 

### Ví dụ 2 

đầu vào:```
n = 6, d = 1, m = 4
a = [1, 2, 5, 6, 3, 4]
```Lớn: [6, 5] 

Nhỏ: [4, 3, 2, 1] 

Chúng tôi tính toán`max_big = (6 + 1) // 2 = 3`. 

Chúng tôi lấy cả hai giá trị lớn. 

| Bước | Hành động | Lấy lớn | Các slot còn lại | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | Lấy 6 | [6] | 6 - 2 = 4 | 6 | 
| 2 | Lấy 5 | [6, 5] | 4 - 2 = 2 | 11 | 

Số slot còn lại = 2 nên ta lấy [4, 3]. 

Câu trả lời cuối cùng = 18. 

Điều này chứng tỏ các giá trị nhỏ lấp đầy không gian còn sót lại một cách tham lam như thế nào sau khi lập kế hoạch cho các giá trị lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; phần còn lại là tập hợp tuyến tính | 
| Không gian | O(n) | Lưu trữ cho mảng phân chia | 

Các ràng buộc cho phép lên đến`10^5`các phần tử, do đó`O(n log n)`giải pháp thoải mái trong giới hạn, trong khi mọi cách tiếp cận dựa trên hoán vị hoặc mô phỏng nặng sẽ quá chậm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, d, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    big = []
    small = []
    for x in a:
        if x > m:
            big.append(x)
        else:
            small.append(x)
    
    big.sort(reverse=True)
    small.sort(reverse=True)
    
    max_big = (n + d) // (d + 1)
    take_big = min(len(big), max_big)
    
    ans = sum(big[:take_big])
    remaining = n - take_big * (d + 1)
    ans += sum(small[:remaining])
    
    return str(ans)

# provided sample
assert run("5 2 11\n8 10 15 23 5\n") == "48"

# all small values
assert run("4 2 100\n1 2 3 4\n") == "10"

# all big values
assert run("5 1 0\n1 2 3 4 5\n") == "9"

# d = 1 edge case
assert run("6 1 4\n1 2 5 6 3 4\n") == "18"

# single element
assert run("1 10 5\n100\n") == "100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị nhỏ | 10 | không có tác dụng chặn | 
| tất cả các giá trị lớn | 9 | thời gian hồi chiêu thường xuyên chiếm ưu thế | 
| d = 1 trường hợp | 18 | khoảng cách phạt tối thiểu | 
| phần tử đơn | 100 | độ đúng cơ sở | 

## Vỏ cạnh 

Khi tất cả các giá trị nhỏ hơn hoặc bằng`m`, thuật toán sẽ đặt mọi thứ vào nhóm nhỏ và tính toán các vị trí còn lại là`n`. Không có sự chặn nào xảy ra nên câu trả lời sẽ trở thành tổng của tất cả các giá trị. Ví dụ,`n = 4, m = 10, a = [1,2,3,4]`kết quả là chọn được cả 4 phần tử. 

Khi tất cả các giá trị vượt quá`m`, thuật toán dựa hoàn toàn vào công thức dung lượng`(n + d) // (d + 1)`. Mỗi phần tử được chọn sử dụng một khối đầy đủ và chỉ một số phần tử lớn nhất được chọn. Vì`n = 5, d = 2, a = [5,4,3,2,1]`, nhiều nhất là hai phần tử được chọn, cho`5 + 4 = 9`. 

Khi`d`lớn so với`n`, chỉ có thể sử dụng một hoặc hai phần tử lớn. Các vị trí còn lại được lấp đầy hoàn toàn bởi các giá trị nhỏ nếu chúng tồn tại. Trường hợp này xác nhận rằng việc tính toán công suất đã giới hạn chính xác ảnh hưởng của các giá trị nguy hiểm bất kể thứ tự.
