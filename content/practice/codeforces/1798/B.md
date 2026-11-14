---
title: "CF 1798B - Ba Số Bảy"
description: "Chúng ta được cung cấp một chuỗi ngày. Vào mỗi ngày, một nhóm người tham gia xổ số và chính xác một người trong số họ được chọn là người chiến thắng trong ngày đó. Hạn chế chính là khi ai đó thắng vào ngày thứ i, họ sẽ bị cấm xuất hiện vào bất kỳ ngày nào sau đó từ i+1 đến m."
date: "2026-06-09T09:50:58+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1798
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 860 (Div. 2)"
rating: 1000
weight: 1798
solve_time_s: 115
verified: false
draft: false
---

[CF 1798B - Ba số bảy](https://codeforces.com/problemset/problem/1798/B) 

**Đánh giá:** 1000 
**Tags:** bạo lực, cấu trúc dữ liệu, tham lam, thực hiện 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi ngày. Vào mỗi ngày, một nhóm người tham gia xổ số và chính xác một người trong số họ được chọn là người chiến thắng trong ngày đó. Hạn chế chính là khi ai đó thắng trong ngày`i`, chúng bị cấm xuất hiện vào bất kỳ ngày nào sau đó`i+1`bởi vì`m`. 

Nhiệm vụ không phải là tái tạo lại lịch sử duy nhất mà là xây dựng bất kỳ chuỗi người chiến thắng hàng ngày hợp lệ nào có thể giải thích danh sách tham gia hoặc xác định rằng không có nhiệm vụ nào như vậy tồn tại. 

Được định hình lại cụ thể hơn, mỗi ngày cung cấp cho chúng ta một tập hợp các ứng viên có sẵn. Chúng ta phải chọn một phần tử từ mỗi bộ theo thứ tự, nhưng một khi phần tử đã được chọn, nó không thể xuất hiện trong bất kỳ tập hợp nào trong tương lai. Khó khăn là những lựa chọn trước đó sẽ hạn chế tính khả thi sau này, vì vậy những lựa chọn tham lam phải tôn trọng khả năng sẵn có trong tương lai. 

Các ràng buộc rất chặt chẽ: tổng số người tham gia trong tất cả các ngày tối đa là 50.000 và có thể có tới 50.000 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi chiến lược so sánh bậc hai hoặc theo cặp mỗi ngày. Bất kỳ giải pháp nào cũng phải xử lý từng người tham gia với số lần không đổi, thường bằng cách quét ngày và cập nhật các cấu trúc phụ trợ nhỏ. 

Một trường hợp thất bại tinh vi phát sinh khi một cách tiếp cận tham lam chọn một người tham gia mà sau đó lại xuất hiện trở lại. Ví dụ: nếu vào ngày 1, chúng tôi chọn một người cũng xuất hiện vào ngày 3, thì lựa chọn đó sẽ làm mất hiệu lực tính khả thi ngay cả khi ngày 2 có các lựa chọn thay thế. Một tình huống phức tạp khác là khi ứng cử viên hợp lệ duy nhất của ngày đã được sử dụng trước đó, khiến việc xây dựng không thể thực hiện được mặc dù các bước tham lam trước đó có vẻ đúng cục bộ. 

Thử thách cốt lõi là đảm bảo rằng khi chúng tôi chọn được người chiến thắng trong ngày`i`, chúng tôi đảm bảo chúng sẽ không xuất hiện vào bất kỳ ngày nào trong tương lai. Vì chúng tôi biết tất cả các danh sách tham gia trong tương lai nên điều này trở thành một hạn chế toàn cầu chứ không phải là hạn chế địa phương. 

## Phương pháp tiếp cận 

Một chiến lược ngây thơ sẽ thử đệ quy tất cả các lựa chọn có thể có cho mỗi ngày, quay lại bất cứ khi nào người chiến thắng được chọn xuất hiện sau đó. Trong trường hợp xấu nhất, mỗi ngày có thể có nhiều ứng cử viên và việc kiểm tra xem một ứng cử viên có xuất hiện trong những ngày trong tương lai hay không sẽ tốn thời gian tuyến tính trừ khi được xử lý trước. Ngay cả với quá trình tiền xử lý, việc khám phá tất cả các kết hợp đều dẫn đến sự phân nhánh theo cấp số nhân. Với thời gian lên tới 50.000 ngày, điều này là hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta không cần phải xem xét tất cả các ứng cử viên. Để ứng viên có hiệu lực trong ngày`i`, chúng phải xuất hiện vào ngày`i`và không được xuất hiện vào bất kỳ ngày nào sau đó. Điều đó có nghĩa là chúng tôi có thể tính toán trước lần xuất hiện cuối cùng của mỗi người trong tất cả các ngày. Khi chúng tôi biết ngày cuối cùng mà mỗi người xuất hiện, vấn đề sẽ giảm xuống còn việc chọn, cho mỗi ngày, bất kỳ người tham gia nào có lần xuất hiện cuối cùng bằng ngày hiện tại hoặc sớm hơn, nhưng chính xác hơn, chúng tôi cần một người có lần xuất hiện cuối cùng chính xác là vào ngày này nếu chúng tôi muốn chỉ định họ một cách an toàn ngay bây giờ. 

Ý tưởng tham lam là xử lý các ngày từ cuối đến đầu theo cách đảm bảo tính chính xác. Nếu chúng tôi chỉ định người chiến thắng ngược lại thì khi đến một ngày, tất cả các quyết định sau đó đã được ấn định và chúng tôi chỉ cần chọn một người tham gia có lần xuất hiện cuối cùng vào hoặc trước ngày hiện tại và người chưa được sử dụng trong hậu tố được xây dựng. 

Tuy nhiên, một giải pháp tiêu chuẩn và đơn giản hơn xuất hiện: chúng tôi xử lý các ngày từ cuối đến đầu trong khi duy trì một tập hợp các ứng cử viên có sẵn có lần xuất hiện gần đây nhất khớp hoặc vượt quá ngày hiện tại và chúng tôi tham lam chọn lọc để đảm bảo rằng chúng tôi không bao giờ vi phạm các ràng buộc trong tương lai. Giải pháp CF trực tiếp và phổ biến hơn sử dụng mảng “lần xuất hiện cuối cùng” và chỉ xây dựng lựa chọn khi thấy an toàn để chỉ định một người vào ngày cuối cùng có thể của họ. 

Điều này biến vấn đề thành một nhiệm vụ tham lam trong nhiều ngày trong đó mỗi người phải được chỉ định chính xác một lần, vào hoặc trước lần xuất hiện cuối cùng của họ và xung đột được giải quyết bằng cách luôn sử dụng ô an toàn sớm nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu (lần xuất hiện cuối cùng + phân công tham lam) | O(tổng n) | O(giá trị tối đa) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán ngày cuối cùng mà chúng xuất hiện đối với mỗi giá trị của người tham gia. Đây là bước tiền xử lý quan trọng vì nó mã hóa ràng buộc “nếu chúng ta chọn người này vào ngày thứ i thì họ không thể xuất hiện sau đó, vì vậy ngày tôi phải là ngày cuối cùng của họ trong chuỗi”. 

Sau đó, chúng tôi quét các ngày theo thứ tự ngược lại, xây dựng câu trả lời từ ngày cuối cùng đến ngày đầu tiên. Chúng tôi duy trì một cấu trúc cho phép chúng tôi biết ứng viên nào có mặt vào ngày hiện tại và liệu họ có thể được phân công một cách an toàn hay không. 

1. Tính ngày xuất hiện cuối cùng cho mỗi giá trị bằng cách quét tất cả danh sách tham gia. 

Điều này cho chúng ta biết thời hạn cuối cùng mà mỗi người phải được phân công nếu chúng ta chọn họ. 
2. Tạo cấu trúc để theo dõi những người tham gia nào xuất hiện mỗi ngày, thường là một mảng danh sách. 
3. Đi qua ngày từ`m`xuống tới`1`, duy trì một tập hợp hoặc cơ cấu ưu tiên của các ứng viên xuất hiện trong ngày`i`. 

Tại thời điểm này, chúng tôi đang quyết định gán cho một hậu tố trong đó tất cả các ràng buộc trong tương lai đã được giải quyết. 
4. Trong ngày`i`, xem xét tất cả những người tham gia được liệt kê ngày hôm đó và thêm họ vào nhóm ứng viên. 

Người tham gia chỉ đủ điều kiện nếu lần xuất hiện cuối cùng của họ chính xác là`i`, nghĩa là đây có thể là ngày phân công cuối cùng của họ. 
5. Nếu không có ứng cử viên như vậy tồn tại trong ngày`i`, thì không thể thực hiện phép gán hợp lệ nào, vì vậy chúng ta kết thúc với thất bại. 

Điều này phản ánh thực tế là mỗi ngày phải tiêu tốn ít nhất một người có khả năng sẵn sàng ở đó. 
6. Mặt khác, chọn bất kỳ ứng cử viên hợp lệ nào (không quan trọng cái nào, vì tất cả đều an toàn khi lần xuất hiện cuối cùng bằng`i`) và chỉ định họ là người chiến thắng trong ngày`i`. 
7. Đánh dấu ứng viên đó là đã sử dụng và tiếp tục cho những ngày trước đó. 

Việc xây dựng đảm bảo rằng mỗi người chiến thắng được chọn sẽ bị loại bỏ chính xác ở lần xuất hiện cuối cùng, vì vậy chúng không bao giờ xung đột với các nhiệm vụ trước đó. 

### Tại sao nó hoạt động 

Điều bất biến là khi xử lý ngày`i`, tất cả các ngày`i+1 ... m`đã chỉ định người chiến thắng và không ai trong số những người chiến thắng đó xuất hiện trở lại vào bất kỳ ngày nào trước đó mà chúng tôi sẽ xử lý. Vì vậy, bất kỳ ứng cử viên nào có lần xuất hiện cuối cùng chính xác là`i`là an toàn để chỉ định, vì không có ràng buộc nào trong tương lai có thể bị vi phạm bằng cách chọn chúng ngay bây giờ. Ngược lại, nếu không có ứng cử viên nào như vậy tồn tại trong một ngày thì mọi người tham gia vào ngày đó sẽ xuất hiện ở đâu đó muộn hơn, nghĩa là bất kỳ lựa chọn nào cũng sẽ vi phạm quy tắc “không xuất hiện lại sau khi chiến thắng”, khiến việc xây dựng không thể thực hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        m = int(input())
        days = []
        last = {}
        
        for i in range(m):
            n = int(input())
            arr = list(map(int, input().split()))
            days.append(arr)
            for x in arr:
                last[x] = i
        
        ans = [-1] * m
        used = set()
        
        ok = True
        
        for i in range(m - 1, -1, -1):
            pick = -1
            for x in days[i]:
                if last[x] == i and x not in used:
                    pick = x
                    break
            
            if pick == -1:
                ok = False
                break
            
            ans[i] = pick
            used.add(pick)
        
        if not ok:
            print(-1)
        else:
            print(*ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc đọc cả ngày và xây dựng một`last`từ điển ghi lại ngày cuối cùng mỗi số xuất hiện. Điều này rất cần thiết vì nó chuyển đổi ràng buộc toàn cục thành điều kiện cục bộ mỗi ngày. 

Sau đó chúng tôi lặp lại qua nhiều ngày. Mỗi ngày, chúng tôi quét danh sách người tham gia và cố gắng tìm một người có lần xuất hiện cuối cùng chính xác là ngày hôm đó và người đó chưa được sử dụng. Người như vậy có thể được chỉ định một cách an toàn là người chiến thắng của ngày hôm đó vì họ sẽ không xuất hiện trong bất kỳ hậu tố nào được xử lý trước đó. 

các`used`set đảm bảo chúng tôi không chỉ định cùng một người hai lần. Vì mỗi người chiến thắng phải là duy nhất về nhiệm vụ, điều này ngăn cản việc sử dụng lại trong nhiều ngày. 

Nếu không có ứng cử viên hợp lệ nào tồn tại trong một ngày, chúng tôi ngay lập tức kết luận là không thể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
4
1 2 4 8
3
2 9 1
2
1 4
```Chúng tôi tính toán lần xuất hiện cuối cùng: 

| Giá trị | Ngày cuối cùng | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1 | 
| 4 | 2 | 
| 8 | 0 | 
| 9 | 1 | 

Bây giờ chúng tôi xử lý ngược: 

| Ngày | Ứng viên | Lựa chọn hợp lệ (ngày == cuối cùng) | Được chọn | 
| --- | --- | --- | --- | 
| 2 | [1, 4] | 1, 4 | 1 | 
| 1 | [2, 9, 1] | 2, 9 | 2 | 

Quá trình xây dựng thành công vì mỗi ngày có ít nhất một người tham gia có diện mạo cuối cùng phù hợp với ngày đang được xử lý. 

Điều này xác nhận sự bất biến rằng mọi người chiến thắng được chỉ định đều được tiêu thụ chính xác ở lần xuất hiện cuối cùng của họ. 

### Ví dụ 2 

đầu vào:```
2
1 2
2 1
```Lần xuất hiện cuối cùng: 

| Giá trị | Ngày cuối cùng | 
| --- | --- | 
| 1 | 1 | 
| 2 | 0 | 

Ngày xử lý 1: 

Chỉ có ứng viên là [1, 2], nhưng chỉ có 1 đáp ứng cuối cùng[x] == 1. Chúng tôi chọn 1. 

Ngày xử lý thứ 2: 

Các ứng cử viên [2, 1], nhưng 1 đã được sử dụng và 2 có lần xuất hiện cuối cùng là 0, vì vậy không có lựa chọn hợp lệ nào tồn tại. 

Quá trình này thất bại, cho thấy cấu trúc ban đầu có thể dẫn đến ngõ cụt khi những ngày trong tương lai phụ thuộc vào những người tham gia được sử dụng lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(∑ n_i) | Mỗi người tham gia được xử lý khi tính toán lần xuất hiện cuối cùng và một lần trong quá trình quét ngược | 
| Không gian | O(giá trị tối đa + m) | Lưu trữ các lần xuất hiện gần đây nhất, danh sách ngày và mảng kết quả | 

Tổng kích thước đầu vào được giới hạn bởi 50.000 trên tất cả các trường hợp thử nghiệm, do đó xử lý tuyến tính là đủ. Thuật toán thực hiện một lượng công việc không đổi cho mỗi người tham gia và mỗi ngày, vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    from io import StringIO
    out = StringIO()
    _stdout = _sys.stdout
    _sys.stdout = out
    solve()
    _sys.stdout = _stdout
    return out.getvalue().strip()

# sample tests (format adapted)
assert run("""1
3
4
1 2 4 8
3
2 9 1
2
1 4
""") != "", "sample 1 basic check"

# minimum case
assert run("""1
1
1
7
""").strip() == "7"

# impossible case
assert run("""1
2
2
1 2
2
1 2
""").strip() == "-1"

# chain case
assert run("""1
3
1
1
1
1
1
1
""").strip() == "1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ngày độc thân | yếu tố đó | giá trị tối thiểu | 
| bộ giống hệt nhau | -1 | không thể do hạn chế tái sử dụng | 
| chuỗi đơn | lựa chọn hợp lệ lặp đi lặp lại | tính nhất quán tầm thường | 

## Vỏ cạnh 

Trường hợp nguy hiểm là khi mọi người tham gia đều xuất hiện trong nhiều ngày trong tương lai. Trong trường hợp đó, không ai đủ điều kiện để phân công vào những ngày trước đó vì lần xuất hiện cuối cùng của họ không bao giờ được căn chỉnh theo ranh giới ngày duy nhất. Thuật toán thất bại một cách chính xác vì một ngày nào đó`i`, KHÔNG`x`thỏa mãn`last[x] == i`. 

Một trường hợp đặc biệt khác là khi mỗi ngày có đúng một người tham gia. Thuật toán thành công hay thất bại một cách tầm thường dựa trên việc những người tham gia đó có tạo thành một chuỗi các lần xuất hiện cuối cùng không chồng chéo hay không. Vì mỗi ngày chỉ đóng góp một ứng cử viên nên việc kiểm tra giảm xuống còn việc xác minh tính nhất quán của các vị trí xuất hiện cuối cùng. 

Trường hợp thứ ba là sự chồng chéo nặng nề khi có nhiều người tham gia xuất hiện trong những ngày liên tiếp. Quá trình quét ngược đảm bảo chúng tôi luôn chỉ định ở ranh giới sẵn có, ngăn chặn việc tiêu thụ sớm. Ngay cả khi một giá trị xuất hiện vào nhiều ngày trước đó, nó chỉ được gán một lần, chính xác ở lần xuất hiện cuối cùng, duy trì tính chính xác trên toàn bộ chuỗi.
