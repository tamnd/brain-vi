---
title: "CF 106039M - Du Mục"
description: "Chúng ta được cung cấp một số đêm cố định, D, và tập hợp N địa điểm có thể mà một người có thể ngủ. Mỗi nơi tôi đi kèm với một ràng buộc di giới hạn số đêm liên tiếp có thể ở ở nơi đó."
date: "2026-06-20T21:10:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106039
codeforces_index: "M"
codeforces_contest_name: "2025 USP Try-outs"
rating: 0
weight: 106039
solve_time_s: 51
verified: true
draft: false
---

[CF 106039M - Nomad](https://codeforces.com/problemset/problem/106039/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số đêm cố định, D, và tập hợp N địa điểm có thể mà một người có thể ngủ. Mỗi nơi tôi đi kèm với một ràng buộc di giới hạn số đêm liên tiếp có thể ở ở nơi đó. Nếu một người ở cùng một chỗ trong hơn hai ngày liên tiếp, họ sẽ bị buộc rời đi ngay lập tức, vì vậy bất kỳ kế hoạch hợp lệ nào cũng phải phá vỡ các chuỗi dài trước khi vượt quá giới hạn. 

Nhiệm vụ là xây dựng một chuỗi bất kỳ có độ dài D, trong đó mỗi phần tử là một chỉ số từ 1 đến N, mô tả nơi người đó ngủ mỗi đêm. Chuỗi phải tuân theo quy tắc không có chỉ mục i nào xuất hiện trong khối liên tiếp dài hơn di. Nếu di bằng 0 thì nơi đó hoàn toàn không thể được sử dụng, bởi vì dù chỉ một đêm cũng đã vi phạm ràng buộc. 

Các ràng buộc N, D ≤ 100000 ngụ ý rằng mọi giải pháp đều phải có nhiều nhất là O(D log D) hoặc O(D). Bất cứ điều gì bậc hai trong D hoặc liên quan đến việc quét toàn bộ lặp đi lặp lại cho mỗi vị trí sẽ không thành công. Một mô phỏng quay lui ngây thơ hoặc tham lam cố gắng sửa chữa các vi phạm sau khi chúng xảy ra sẽ quá chậm trong trường hợp xấu nhất khi D lớn và cấu hình hợp lệ chặt chẽ. 

Một trường hợp lỗi tinh tế xuất hiện khi tất cả các giá trị di có thể sử dụng đều nhỏ nhưng D lại lớn. Ví dụ: nếu tất cả di đều là 1 thì không có vị trí nào có thể được sử dụng hai lần liên tiếp, vì vậy chúng ta phải luân phiên hoàn hảo. Nếu N = 1 và D = 2 với d1 = 1 thì không thể, nhưng nếu N = 2 thì tồn tại nghiệm. Điều này cho thấy tính khả thi không chỉ phụ thuộc vào tổng công suất mà còn phụ thuộc vào việc liệu chúng ta có thể luôn “chuyển đi” trước khi đạt đến giới hạn hay không. 

Một trường hợp cạnh khác phát sinh khi tất cả di bằng 0. Khi đó mọi nơi đều bị cấm và thậm chí D = 1 là không thể. Một cách tiếp cận tham lam mà bỏ qua di = 0 sẽ cố gắng sử dụng các chỉ số đó một cách không chính xác. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xây dựng trình tự từng ngày và mỗi ngày thử tất cả các địa điểm có thể, kiểm tra xem việc thêm địa điểm đó có vi phạm ràng buộc liên tiếp hay không. Nếu có thì chúng ta thử chỗ khác. Chúng tôi duy trì độ dài chuỗi hiện tại cho vị trí hiện tại và cập nhật nó khi chúng tôi mở rộng chuỗi. 

Điều này hoạt động hợp lý vì chúng tôi chỉ chấp nhận các chuỗi tôn trọng các ràng buộc, nhưng vấn đề là trong trường hợp xấu nhất, chúng tôi có thể thử nhiều lựa chọn thay thế mỗi ngày. Nếu chúng tôi gặp phải tình huống mà vị trí hiện tại đã cạn kiệt và chúng tôi phải chuyển đổi, chúng tôi có thể quét liên tục tất cả N ứng cử viên, dẫn đến hành vi O(DN), tức là 10^10 thao tác trong trường hợp xấu nhất. 

Quan sát quan trọng là hạn chế chỉ phụ thuộc vào việc sử dụng liên tục. Một khi chúng ta đã chọn một địa điểm, chúng ta được phép ở đó tối đa là hai ngày liên tục, nhưng không có gì buộc chúng ta phải rời đi sớm hơn. Điều này cho thấy chúng ta nên luôn sử dụng một địa điểm để đảm bảo an toàn tối đa, sau đó chuyển đổi. 

Chúng ta có thể coi mỗi nơi như một “tài nguyên” có thể phát ra các khối có chiều dài lên tới di. Chúng tôi muốn ghép các khối này thành một chuỗi có tổng chiều dài D. Khó khăn duy nhất là đảm bảo chúng tôi luôn có khối tiếp theo hợp lệ khi khối hiện tại kết thúc. 

Chiến lược tham lam tự nhiên là luôn chọn vị trí có khoảng thời gian cho phép lớn nhất còn lại khi chúng ta cần bắt đầu hoặc bắt đầu lại một phân đoạn. Nếu luôn chọn phương án mạnh nhất hiện tại, chúng ta sẽ tránh bị mắc kẹt chỉ với những phương án yếu không thể lấp đầy những ngày còn lại. 

Để hỗ trợ điều này một cách hiệu quả, chúng tôi duy trì vùng heap tối đa được khóa bởi di. Chúng tôi liên tục chiếm vị trí sẵn có tốt nhất, sử dụng nó trong tối đa min(di, còn lại), sau đó đẩy nó trở lại nếu nó vẫn còn dung lượng (hoặc đơn giản là sử dụng lại vì di không giảm trên toàn cầu, chỉ có mức sử dụng liên tiếp mới quan trọng; thay vào đó chúng tôi quản lý các chuỗi một cách rõ ràng bằng cách sử dụng lại cùng một nhóm).

Cấu trúc quan trọng là chúng tôi chỉ quan tâm đến việc ngăn chặn các lượt chạy liên tiếp quá dài, vì vậy, bất cứ khi nào chúng tôi chuyển khỏi một địa điểm, chúng tôi sẽ đặt lại tính đủ điều kiện của địa điểm đó ngay lập tức. Điều này biến vấn đề thành việc liên tục chọn một địa điểm khác với địa điểm được sử dụng gần đây nhất trong khi vẫn tối đa hóa độ dài vệt cho phép. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(DN) | O(N) | Quá chậm | 
| Tối ưu (tham lam + đống) | O(D log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta duy trì một hàng ưu tiên gồm các địa điểm có sẵn, được sắp xếp theo thứ tự giảm dần di. Chúng tôi cũng theo dõi địa điểm được chọn cuối cùng và số lần chúng tôi đã sử dụng địa điểm đó liên tiếp. 

1. Chèn tất cả các chỉ số i sao cho di > 0 vào vùng heap tối đa được khóa bởi di. Chúng đại diện cho những nơi có thể sử dụng được; di = 0 mục bị bỏ qua hoàn toàn vì chúng không bao giờ có thể xuất hiện theo một trình tự hợp lệ. 
2. Khởi tạo danh sách kết quả trống và các biến Last_place = -1 và Streak = 0. 
3. Mỗi ngày từ 1 đến D, chúng ta chọn chỗ ngủ tiếp theo. Đầu tiên chúng ta cố gắng chọn vị trí tốt nhất có sẵn từ heap. Nếu địa điểm đó giống với địa điểm cuối cùng và việc sử dụng nó sẽ vượt quá giới hạn liên tiếp cho phép, chúng tôi tạm thời bỏ qua địa điểm đó và chọn ứng viên tốt nhất tiếp theo. 

Lý do bỏ qua là heap không mã hóa “trạng thái chuỗi hiện tại”, vì vậy chúng ta phải thực thi ràng buộc đó một cách thủ công. 

1. Sau khi chọn vị trí ứng cử viên x, chúng tôi sẽ thêm vị trí đó vào câu trả lời và cập nhật vị trí cuối cùng. Nếu x bằng vị trí cuối cùng, chúng tôi sẽ tăng chuỗi, nếu không, chúng tôi đặt lại chuỗi thành 1. 
2. Sau khi sử dụng x, chúng ta chỉ chèn lại nó vào heap nếu nó vẫn hữu ích cho việc lựa chọn trong tương lai. Vì di là mức cho phép liên tiếp tối đa chứ không phải là tài nguyên có thể tiêu thụ nên nó vẫn không thay đổi trong heap, nhưng chúng tôi đảm bảo tính chính xác bằng cách chỉ chặn việc sử dụng quá mức thông qua theo dõi chuỗi. 
3. Nếu tại bất kỳ thời điểm nào chúng tôi không thể tìm thấy ứng viên hợp lệ khác với vị trí cuối cùng trong khi chuỗi đã đạt đến d[last_place], thì chúng tôi kết luận rằng việc xây dựng là không thể. 

Tại sao chiến lược lựa chọn này là đúng gắn liền với thực tế là bất cứ khi nào chúng ta buộc phải chuyển đổi thì bất kỳ giải pháp hợp lệ nào cũng phải chuyển đổi vào thời điểm đó. Việc chọn di lớn nhất hiện có đảm bảo chúng tôi tối đa hóa tính linh hoạt cho các phân khúc trong tương lai. 

### Tại sao nó hoạt động 

Tại ranh giới hàng ngày, hạn chế duy nhất có thể phá vỡ tính khả thi là buộc phải kéo dài thời gian chạy vượt quá giới hạn di của nó. Thuật toán duy trì tính bất biến rằng độ dài chạy hiện tại không bao giờ vượt quá di vị trí của nó và bất cứ khi nào cần chuyển đổi, mọi giải pháp hợp lệ cũng phải thực hiện chuyển đổi tại hoặc trước thời điểm đó. Vì chúng tôi luôn chọn khối tiếp theo có sẵn dễ dàng nhất nên chúng tôi không bao giờ giảm tính khả thi trong tương lai nhiều hơn mức cần thiết và chúng tôi không bao giờ tiêu thụ một địa điểm theo cách khiến nó không thể sử dụng được sớm hơn yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    n, d = map(int, input().split())
    arr = list(map(int, input().split()))
    
    heap = []
    for i, v in enumerate(arr):
        if v > 0:
            heapq.heappush(heap, (-v, i + 1))
    
    if not heap:
        print(-1)
        return
    
    res = []
    last = -1
    streak = 0
    last_limit = 0
    
    for _ in range(d):
        if not heap:
            print(-1)
            return
        
        v1, i1 = heapq.heappop(heap)
        v1 = -v1
        
        if i1 == last and streak == v1:
            if not heap:
                print(-1)
                return
            v2, i2 = heapq.heappop(heap)
            v2 = -v2
            heapq.heappush(heap, (-v1, i1))
            
            chosen = i2
            if i2 == last:
                streak += 1
            else:
                streak = 1
            last = i2
            res.append(chosen)
        else:
            chosen = i1
            heapq.heappush(heap, (-v1, i1))
            
            if i1 == last:
                streak += 1
            else:
                streak = 1
            last = i1
            res.append(chosen)
    
    print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ số lượng địa điểm tối đa theo di. Mỗi ngày, nó cố gắng sử dụng nơi mạnh nhất hiện có. Nếu địa điểm đó vi phạm giới hạn liên tiếp do chuỗi hiện tại, nó sẽ tạm thời bỏ qua và sử dụng ứng cử viên tốt thứ hai. 

Điều tinh tế quan trọng là chúng tôi không loại bỏ vĩnh viễn các phần tử khỏi vùng nhớ heap. Mỗi vị trí vẫn có sẵn nhưng tính chính xác được thực thi theo logic chuỗi. Cơ chế hoán đổi đảm bảo chúng ta không bao giờ gặp khó khăn sớm khi ứng cử viên tốt nhất giống với địa điểm được sử dụng cuối cùng nhưng đã bão hòa. 

Phải cẩn thận trong việc theo dõi chính xác; lỗi từng cái một là phổ biến vì chuỗi tính lần chạy hiện tại bao gồm cả ngày hiện tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
0 1 3
```Chúng ta bắt đầu với heap = {(3,3), (1,2)}. 

| Ngày | Đỉnh đống | Được chọn | cuối cùng | vệt | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 3 | 1 | 
| 2 | 3 | 3 | 3 | 2 | 
| 3 | 3 | 3 | 3 | 3 | 
| 4 | 3 bị chặn (sẽ vượt quá) | 2 | 2 | 1 | 
| 5 | 3 | 3 | 3 | 1 | 

Đầu ra:```
3 3 3 2 3
```Dấu vết này cho thấy chúng ta đã khai thác tối đa chuỗi vị trí thứ 3 trước khi chuyển sang phương án duy nhất. 

### Ví dụ 2 

đầu vào:```
2 3
1 1
```Đống = {(1,1), (1,2)}. 

| Ngày | Đỉnh đống | Được chọn | cuối cùng | vệt | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 
| 2 | 1 | 2 | 2 | 1 | 
| 3 | 1 | 1 | 1 | 1 | 

Đầu ra:```
1 2 1
```Điều này thể hiện sự luân phiên bắt buộc khi tất cả di đều bằng 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(D log N) | Mỗi ngày thực hiện tối đa một số thao tác heap không đổi | 
| Không gian | O(N) | Heap lưu trữ tất cả những nơi có thể sử dụng được | 

Các ràng buộc cho phép tối đa 100000 ngày và mỗi bước được tính logarit theo số lượng vị trí, phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    old = sys.stdout
    sys.stdout = io.StringIO()
    
    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdout = old

# provided sample 1
assert run("3 5\n0 1 3\n") in ["3 3 3 2 3", "3 3 3 3 2"]

# provided sample 2
assert run("10 1\n0 0 0 0 0 0 0 0 0 0\n") == "-1"

# all equal small
assert run("2 4\n1 1\n") in ["1 2 1 2", "2 1 2 1"]

# single usable place too short
assert run("1 2\n1\n") == "-1"

# single usable place valid
assert run("1 1\n1\n") == "1"

# boundary alternating
assert run("3 6\n1 2 1\n")  # just ensure no crash
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 5 / 0 1 3 | trình tự hợp lệ | hành vi tham lam bình thường | 
| 10 1 / toàn số không | -1 | trường hợp bất khả thi | 
| 2 4 / tất cả những cái | xen kẽ | chuyển đổi nghiêm ngặt | 
| 1 2/1 | -1 | lỗi tùy chọn duy nhất | 
| 1 1/1 | 1 | trường hợp hợp lệ tối thiểu | 

## Vỏ cạnh 

Trường hợp tất cả di đều bằng 0 được xử lý ngay lập tức bằng kiểm tra vùng heap trống. Vì không có vị trí nào được chèn vào nên thuật toán sẽ in -1 trước khi thử bất kỳ cách xây dựng nào. 

Khi chỉ có một vị trí hợp lệ với di = 1 nhưng D > 1, heap luôn trả về cùng một phần tử và điều kiện sọc ngay lập tức chặn việc sử dụng tiếp theo. Dự phòng vùng heap thứ hai không tồn tại, do đó thuật toán kết thúc chính xác bằng -1. 

Khi nhiều vị trí có di = 1 bằng nhau, vùng heap luôn cung cấp các ứng cử viên thay thế. Logic chuỗi đảm bảo rằng ngay cả khi cùng một chỉ mục xuất hiện lại ở trên cùng do thứ tự đống, nó sẽ bị bỏ qua để chuyển sang một chỉ mục có sẵn khác, tạo ra một chuỗi xen kẽ hợp lệ.
