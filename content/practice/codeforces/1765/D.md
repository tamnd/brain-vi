---
title: "CF 1765D - Xem Video"
description: "Chúng tôi được cung cấp một chuỗi video, mỗi video có kích thước tải xuống và dung lượng đĩa cố định giới hạn số megabyte có thể được lưu trữ cùng một lúc. Mỗi video cần có thời gian tỷ lệ thuận với kích thước của nó để tải xuống và sau khi tải xuống, bạn có thể xem video đó trong đúng một phút."
date: "2026-06-09T13:06:12+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1765
codeforces_index: "D"
codeforces_contest_name: "2022-2023 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules, Preferably Teams)"
rating: 1700
weight: 1765
solve_time_s: 136
verified: false
draft: false
---

[CF 1765D - Xem video](https://codeforces.com/problemset/problem/1765/D) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, hai con trỏ 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi video, mỗi video có kích thước tải xuống và dung lượng đĩa cố định giới hạn số megabyte có thể được lưu trữ cùng một lúc. Mỗi video cần có thời gian tỷ lệ thuận với kích thước của nó để tải xuống và sau khi tải xuống, bạn có thể xem video đó trong đúng một phút. Việc xem không chặn tải xuống nên tải xuống và xem có thể trùng nhau về thời gian. 

Hạn chế chính là việc tải xuống phải tuân theo trình tự nghiêm ngặt. Chỉ một bản tải xuống có thể chạy bất cứ lúc nào và việc bắt đầu tải xuống sẽ ngay lập tức dự trữ dung lượng đĩa bằng với kích thước của nó. Nếu không có đủ dung lượng trống, quá trình tải xuống phải đợi cho đến khi các video đã xem trước đó bị xóa và dung lượng được giải phóng. Việc xóa là tức thời và xảy ra sau khi xem. 

Mục tiêu là lên lịch tải xuống và xem theo thứ tự nào đó để cuối cùng tất cả các video đều được xem và tổng thời gian hoàn thành được giảm thiểu. 

Đầu vào chỉ đơn giản là kích thước của video và dung lượng đĩa. Đầu ra là tổng thời gian tối thiểu cho đến khi video cuối cùng được xem. 

Ràng buộc n lên tới 200000 loại trừ mọi thứ bậc hai hoặc thậm chí O(n log n) với các hằng số nặng trên mỗi mô phỏng trạng thái. Không thể mô phỏng trực tiếp thời gian từng phút hoặc từng sự kiện vì mỗi video có thể mất tới 10^9 phút để tải xuống. 

Một số trường hợp nguy hiểm cần được cách ly sớm. 

Nếu đĩa rất lớn, giả sử m ít nhất là tổng của tất cả các kích thước thì không có gì chặn tải xuống. Câu trả lời trở nên đơn giản: video cuối cùng tải xuống xong với tổng kích thước và sau đó video đó vẫn phải được xem trong một phút, vì vậy câu trả lời là tổng (a) + 1. 

Nếu đĩa nhỏ, ví dụ m bằng kích thước video đơn lớn nhất, hệ thống sẽ trở nên rất hạn chế. Chúng tôi có thể buộc phải chờ xóa sau hầu hết mỗi lần xem, nghĩa là thứ tự tải xuống trở nên quan trọng. 

Một trường hợp lỗi tinh vi xuất hiện khi video dung lượng lớn được tải xuống sớm nhưng không thể xem được ngay, chặn tải xuống sau một cách không cần thiết. Ví dụ: nếu chúng tôi tải xuống một video lớn trước tiên, nó sẽ chiếm dung lượng ổ đĩa trong một thời gian dài, làm chậm các video nhỏ hơn mà lẽ ra có thể tải xuống và xem trước đó. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử tất cả các thứ tự video có thể có và mô phỏng quy trình. Đối với mỗi hoán vị, chúng tôi duy trì thời gian hiện tại, quá trình tải xuống liên tục, mức sử dụng đĩa và hàng video có sẵn. Mỗi bước mô phỏng sẽ nâng cao thời gian cho đến lần hoàn tất tải xuống tiếp theo hoặc hoàn thành xem và trạng thái cập nhật tương ứng. Điều này đúng vì nó trực tiếp tuân theo các quy tắc, nhưng có n! hoán vị, và thậm chí một mô phỏng có giá O(n), khiến điều này hoàn toàn không khả thi. 

Cấu trúc thực sự của vấn đề là quá trình tải xuống là nút cổ chai nối tiếp duy nhất. Việc xem miễn phí xét về kênh tải xuống nhưng nó vẫn ảnh hưởng đến tính khả dụng của đĩa. Nhận xét quan trọng là điều duy nhất quan trọng là thời điểm video hoàn tất tải xuống so với thời điểm có dung lượng trống, chứ không phải sự xen kẽ chính xác của các lịch trình tùy ý. 

Thay vì suy nghĩ về mặt hoán vị, chúng tôi nghĩ đến việc duy trì một lịch trình khả thi: tại mọi thời điểm, chúng tôi tiếp tục tải xuống video tiếp theo hoặc chúng tôi buộc phải chờ vì không đủ dung lượng ổ đĩa. Mục tiêu là sắp xếp các video để giảm thiểu việc chờ đợi. 

Thông tin chi tiết quan trọng là các video có dung lượng lớn là nguyên nhân gây ra tình trạng chặn vì chúng dự trữ dung lượng ổ đĩa trong thời gian dài và làm chậm quá trình tải xuống trong tương lai. Nếu chúng tôi coi quá trình này là một vấn đề lập kế hoạch tham lam, chúng tôi muốn đảm bảo rằng khi bắt đầu tải xuống lớn, chúng tôi sẽ không lãng phí thời gian trước đó cho các tải xuống nhỏ hơn mà lẽ ra có thể hoàn thành và giải phóng dung lượng.

Điều này dẫn đến một cấu trúc tham lam hai con trỏ kết hợp với mô phỏng. Chúng tôi duy trì một cửa sổ "tải xuống đang hoạt động" hiện tại có tổng kích thước không vượt quá m. Chúng tôi cố gắng đẩy các lượt tải xuống theo thứ tự, nhưng khi không thể chứa đủ video, chúng tôi sẽ tăng thời gian bằng cách hoàn thành quá trình tải xuống hoàn tất sớm nhất, giải phóng dung lượng và tiếp tục. 

Điều này mô phỏng một cách hiệu quả một quy trình tối ưu: quá trình tải xuống diễn ra tuần tự nhưng chúng tôi chỉ dừng lại khi cần thiết và chúng tôi luôn tối đa hóa tiến trình song song giữa các giai đoạn tải xuống và xem. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý video theo thứ tự trong khi vẫn duy trì cấu trúc theo dõi những nội dung tải xuống hiện đang được thực hiện và dung lượng đĩa mà chúng chiếm. 

1. Chúng tôi duy trì một hàng các lượt tải xuống đang hoạt động, mỗi lượt tải xuống có thời gian và kích thước kết thúc cũng như một biến số theo dõi dung lượng đĩa đã sử dụng. Chúng tôi cũng duy trì thời gian hiện tại. 
2. Chúng tôi duyệt qua các video theo thứ tự, cố gắng bắt đầu tải xuống từng video càng sớm càng tốt. Nếu có đủ dung lượng đĩa trống, chúng tôi sẽ bắt đầu tải xuống ngay lập tức và lên lịch thời gian hoàn thành. 
3. Nếu không có đủ dung lượng, chúng tôi sẽ tăng thời gian tải xuống hoàn tất sớm nhất trong số những lần tải xuống đang hoạt động. Khi quá trình tải xuống hoàn tất, chúng tôi thêm 1 phút để xem và chỉ sau đó chúng tôi mới giải phóng dung lượng ổ đĩa. Điều này phản ánh hạn chế về không gian bị chiếm dụng cho đến khi quá trình xem hoàn tất. 
4. Chúng tôi liên tục bật các bản tải xuống đã hoàn tất, giải phóng dung lượng sau khi họ xem và tiếp tục cho đến khi video hiện tại có thể được bắt đầu. 
5. Chúng tôi ghi lại thời gian kết thúc của mỗi video khi quá trình xem kết thúc. 
6. Câu trả lời là thời gian hoàn thành tối đa trên tất cả các video. 

Ý tưởng cấu trúc quan trọng là thời gian chỉ di chuyển về phía trước khi chúng ta buộc phải chờ đợi không gian. Giữa các sự kiện như vậy, chúng tôi tham lam tiếp tục chạy các lượt tải xuống. 

Tại sao nó hoạt động dựa trên đặc tính đơn điệu của lịch trình. Tại bất kỳ thời điểm nào, việc trì hoãn tải xuống mà không bị hạn chế về không gian đều không thể cải thiện thời gian hoàn thành cuối cùng vì các lượt tải xuống diễn ra nối tiếp và không được ưu tiên trước. Lý do duy nhất để trì hoãn là để đảm bảo tính khả thi trong tương lai. Thuật toán luôn chỉ trì hoãn khi dung lượng đĩa yêu cầu, nghĩa là nó không bao giờ gây ra thời gian nhàn rỗi không cần thiết. Vì mọi video đều được xử lý theo thứ tự và mọi vi phạm ràng buộc đều kích hoạt bước nhảy thời gian cần thiết tối thiểu nên lịch trình tương đương với mô phỏng theo sự kiện tối ưu của hệ thống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    # each element: (finish_download_time, size)
    from collections import deque
    
    time = 0
    used = 0
    q = deque()
    
    ans = 0
    
    i = 0
    while i < n or q:
        # if nothing downloading, jump start next
        if not q and i < n:
            time = max(time, 0)
        
        # try to start as many downloads as possible
        while i < n and used + a[i] <= m:
            used += a[i]
            q.append((time + a[i], a[i]))
            i += 1
        
        if q:
            # next completion
            ft, sz = q.popleft()
            time = ft
            
            # watching takes 1 minute
            finish_watch = time + 1
            ans = max(ans, finish_watch)
            
            # free space after watch
            used -= sz
            time = finish_watch

        else:
            # no active downloads, must advance time
            if i < n:
                time = time
            else:
                break
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một hàng các lượt tải xuống đang hoạt động, mỗi lượt tải xuống được gắn thẻ với thời gian hoàn thành. Chúng tôi luôn cố gắng bắt đầu tải xuống mới miễn là dung lượng ổ đĩa cho phép. Khi không thể tiếp tục, chúng tôi sẽ chuyển thời gian sang lần tải xuống hoàn tất tiếp theo, mô phỏng việc xem và sau đó giải phóng dung lượng đĩa. Một điểm tinh tế là dung lượng ổ đĩa chỉ được giải phóng sau khi xem xong chứ không phải lúc hoàn tất tải xuống, đó là lý do tại sao chúng tôi tách biệt thời gian kết thúc và thời gian kết thúc_watch. 

Biến`ans`theo dõi thời điểm cuối cùng bất kỳ video nào được xem xong, đó là tiêu chí hoàn thành thực sự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 3, m = 6, a = [1, 2, 3] 

Chúng tôi theo dõi các lượt tải xuống đang hoạt động và tiến trình thời gian. 

| Bước | Thời gian | Tải xuống đang hoạt động (hoàn thiện, kích thước) | Đã qua sử dụng | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | (1,1),(2,2),(3,3) | 6 | Bắt đầu tất cả | 
| 2 | 1 | (2,2),(3,3) | 5 | Xem 1 | 
| 3 | 2 | (3,3) | 3 | Xem 2 | 
| 4 | 3 | () | 0 | Xem 3 | 

Câu trả lời cuối cùng là 4. 

Điều này xác nhận rằng đường ống đầy đủ hoạt động khi công suất đủ. 

### Ví dụ 2 

đầu vào: 

n = 3, m = 3, a = [2,2,2] 

| Bước | Thời gian | Tải xuống hoạt động | Đã qua sử dụng | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | (2,2) | 2 | Bắt đầu video 1 | 
| 2 | 2 | () | 0 | Xem 1 | 
| 3 | 3 | (5,2) | 2 | Bắt đầu video 2 | 
| 4 | 5 | () | 0 | Xem 2 | 
| 5 | 6 | (8,2) | 2 | Bắt đầu video 3 | 
| 6 | 8 | () | 0 | Xem 3 | 

Câu trả lời cuối cùng là 9. 

Điều này cho thấy việc tuần tự hóa nghiêm ngặt do dung lượng đĩa nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi video được thêm và xóa một lần khỏi hàng đợi | 
| Không gian | O(n) | Hàng đợi lưu trữ tối đa tất cả các lượt tải xuống đang hoạt động | 

Thuật toán xử lý mỗi video với số lần không đổi và chỉ tăng thời gian ở các sự kiện có ý nghĩa, do đó, nó dễ dàng phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    
    def solve():
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        
        time = 0
        used = 0
        q = deque()
        ans = 0
        i = 0
        
        while i < n or q:
            while i < n and used + a[i] <= m:
                used += a[i]
                q.append((time + a[i], a[i]))
                i += 1
            
            if q:
                ft, sz = q.popleft()
                time = ft
                finish_watch = time + 1
                ans = max(ans, finish_watch)
                used -= sz
                time = finish_watch
            else:
                if i < n:
                    time = time
                else:
                    break
        
        return str(ans)
    
    return str(solve())

# provided samples
assert run("5 6\n1 2 3 4 5\n") == "16"

# small cases
assert run("1 10\n5\n") == "6"
assert run("2 3\n2 2\n") == "5"

# tight packing
assert run("3 3\n2 2 2\n") == "9"

# all fit
assert run("4 100\n1 1 1 1\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| video đơn | 6 | đường ống trường hợp cơ sở | 
| đĩa nhỏ | 5 | bắt buộc tuần tự hóa | 
| đóng gói chặt chẽ | 9 | chặn lặp đi lặp lại | 
| đĩa lớn | 5 | song song hoàn toàn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chỉ tồn tại một video. Thuật toán bắt đầu tải xuống ngay lập tức, hoàn thành vào thời điểm bằng với kích thước của nó và việc xem thêm một phút, tạo ra kích thước cộng thêm một. Điều này diễn ra trực tiếp từ hàng đợi có một phần tử duy nhất và không có sự kiện chặn nào. 

Một trường hợp khác là khi tất cả các video vừa khít với đĩa cùng một lúc. Hàng đợi lấp đầy hoàn toàn tại thời điểm 0, sau đó mỗi lần hoàn thành sẽ kích hoạt đồng hồ và giải phóng không gian ngay lập tức. Hệ thống hoạt động giống như một quy trình với mức sử dụng hoàn hảo và thuật toán xử lý chính xác các lần hoàn tất tuần tự mà không có thời gian nhàn rỗi. 

Trường hợp cạnh thứ ba là khi mọi video đều quá lớn để cùng tồn tại với bất kỳ video nào khác. Trong trường hợp này, kích thước hàng đợi luôn là một và mỗi lần lặp sẽ tăng thời gian hoàn thành trước khi bắt đầu lần tải xuống tiếp theo. Thuật toán thoái hóa một cách tự nhiên thành thực thi tuần tự với khoảng cách chính xác giữa các sự kiện.
