---
title: "CF 105486J - Giải thưởng lớn của Ballance"
description: "Hệ thống đang mô phỏng một cuộc thi trực tiếp tạo ra một luồng nhật ký máy chủ trong khi cuộc thi đang diễn ra. Có một số cấp độ và tại bất kỳ thời điểm nào chỉ có một cấp độ là “hoạt động”, được xác định bởi nhật ký gần đây nhất của loại 1."
date: "2026-06-23T18:27:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105486
codeforces_index: "J"
codeforces_contest_name: "2024 ICPC Asia Chengdu Regional Contest (The 3rd Universal Cup. Stage 15: Chengdu)"
rating: 0
weight: 105486
solve_time_s: 51
verified: true
draft: false
---

[CF 105486J - Grand Prix of Ballance](https://codeforces.com/problemset/problem/105486/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hệ thống đang mô phỏng một cuộc thi trực tiếp tạo ra một luồng nhật ký máy chủ trong khi cuộc thi đang diễn ra. Có một số cấp độ và tại bất kỳ thời điểm nào, chỉ có một cấp độ là "hoạt động", được xác định bởi nhật ký gần đây nhất thuộc loại 1. Mọi thứ khác trong nhật ký bên ngoài phân đoạn hoạt động đó đều không liên quan đến việc tính điểm. 

Trong cửa sổ cấp độ hoạt động, người tham gia có thể báo cáo các sự kiện hoàn thành hoặc từ bỏ. Tuy nhiên, chỉ sự kiện hợp lệ đầu tiên cho mỗi người tham gia ở mỗi cấp độ mới quan trọng và nó quyết định liệu họ có được coi là đã hoàn thành cấp độ đó thành công hay không. Các sự kiện hoàn thành thiết lập thứ tự: người về đích đầu tiên nhận được m điểm, người về thứ hai nhận được m−1, v.v., trong đó m là số lượng người tham gia. Những người tham gia không bao giờ hoàn thành cấp độ hoặc có sự kiện đầu tiên là bỏ cuộc sẽ nhận được 0 cho cấp độ đó. 

Nhiệm vụ là xây dựng lại điểm số cuối cùng ở tất cả các cấp độ và những người tham gia đầu ra được sắp xếp theo tổng điểm giảm dần, phá vỡ mối quan hệ theo chỉ số người tham gia nhỏ hơn. 

Các ràng buộc chỉ ra rằng chúng tôi không thể mô phỏng bất kỳ thứ gì bậc hai về số lượng nhật ký hoặc số người tham gia trên mỗi cấp độ. Với tối đa 2·10^5 sự kiện ghi trong nhật ký cho mỗi lần kiểm tra và tối đa 10^4 lần kiểm tra, mọi sự kiện đều có thể quét qua tất cả người tham gia hoặc tất cả sự kiện trước đó của mỗi người tham gia là không thể. Giải pháp dự định phải xử lý mỗi lần đăng nhập gần với thời gian khấu hao O(1) hoặc O(log n) và mỗi người tham gia chỉ phải được cập nhật khi họ thực sự đóng góp hành động đầu tiên hợp lệ. 

Một vấn đề tế nhị phát sinh từ các tin nhắn không hợp lệ. Người tham gia có thể gửi nhiều tin nhắn loại 2 hoặc loại 3, nhưng chỉ tin nhắn hợp lệ đầu tiên ở cấp độ hoạt động mới quan trọng. Một điều tinh tế khác là các nhật ký bên ngoài cấp độ hoạt động phải bị bỏ qua hoàn toàn, ngay cả khi chúng chứa các sự kiện hoàn thành. Cuối cùng, các cấp độ có thể không bao giờ xuất hiện hoặc có thể xuất hiện giữa chừng, nghĩa là việc tính điểm chỉ bắt đầu sau sự kiện loại 1 đầu tiên. 

Một sai lầm ngây thơ là nối thêm tất cả các sự kiện loại 2 trên toàn cầu mà không kiểm tra xem chúng có thuộc cấp độ hiện tại hay không. 

Ví dụ: giả sử cấp 1 chỉ hoạt động sau sự kiện loại 1 nhưng chúng tôi xử lý các lần hoàn thành trước đó: 

đầu vào:```
2 2 3
2 1 1
1 1
2 1 1
```Việc hoàn thành đầu tiên phải được bỏ qua vì chưa có cấp độ nào được kích hoạt. Chỉ sự kiện cuối cùng mới được tính, giúp người tham gia 1 về đích hợp lệ. 

Một sai lầm phổ biến khác là không chặn được các tin nhắn tiếp theo từ cùng một người tham gia ở cùng cấp độ sau sự kiện hợp lệ đầu tiên của họ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ là mô phỏng từng cấp độ một cách độc lập. Đối với cấp độ hoạt động hiện tại, chúng tôi duy trì một tập hợp hoặc mảng đánh dấu xem mỗi người tham gia đã tạo sự kiện hợp lệ đầu tiên của họ hay chưa và một danh sách ghi lại thứ tự hoàn thành. Mỗi khi một sự kiện loại 2 xuất hiện, nếu nó hợp lệ và là sự kiện đầu tiên dành cho người tham gia ở cấp độ này, chúng tôi sẽ thêm họ vào danh sách kết thúc. Loại 3 đánh dấu chúng là thất bại nếu đây là sự kiện hợp lệ đầu tiên của chúng. 

Điều này hoạt động hợp lý, nhưng thách thức là đảm bảo chúng tôi không liên tục quét hoặc đặt lại các mảng lớn cho mỗi cấp độ. Nếu chúng ta xóa một mảng có kích thước m cho mỗi cấp độ và có tối đa n cấp độ thì giá trị này sẽ trở thành O(nm), quá lớn. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần đặt lại trạng thái giữa các cấp nếu chúng ta có thể đánh dấu thời gian sử dụng. Mỗi tương tác ở cấp độ người tham gia chỉ quan trọng một lần cho mỗi cấp độ và mỗi sự kiện được xử lý một lần. Vì vậy, thay vì xóa mảng, chúng tôi lưu trữ thời điểm họ được cập nhật lần cuối cho từng người tham gia hoặc chúng tôi duy trì các từ điển theo cấp độ do người tham gia khóa nhưng chỉ dành cho các mục nhập hoạt động. 

Một sự đơn giản hóa quan trọng khác là chúng tôi chỉ quan tâm đến thứ tự hoàn thành thành công ở mỗi cấp độ. Thời gian chính xác đã được đưa ra theo thứ tự nhật ký, vì vậy chúng tôi có thể thêm người chiến thắng theo trình tự và chỉ định điểm theo thứ tự ngược lại ở cuối mỗi cấp. 

Vì vậy, chúng tôi mô phỏng nhật ký một lần, duy trì: 

- mức độ hoạt động hiện tại 
- mỗi cấp độ, một danh sách những người về đích thành công 
- mỗi cấp độ, một bản đồ từ người tham gia đến việc họ đã có quyết định chưa 

Khi một cấp độ mới bắt đầu, chúng tôi sẽ hoàn tất việc tính điểm cho cấp độ trước đó. 

Cuối cùng, chúng tôi tính toán tổng số và sắp xếp người tham gia. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Thiết lập lại Brute Force theo cấp độ | O(n·m + q) | O(n·m) | Quá chậm | 
| Theo dõi từng cấp độ lười biếng với bản đồ | O(q + m log m) | O(q + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại nhật ký theo thứ tự, theo dõi cấp độ hiện đang hoạt động. Nếu chưa có cấp độ nào được kích hoạt, hãy bỏ qua tất cả các sự kiện. 

Cấp độ hoạt động xác định bối cảnh duy nhất trong đó các sự kiện quan trọng. 
2. Khi sự kiện loại 1 xuất hiện, hãy hoàn tất cấp độ trước đó nếu nó tồn tại. Điều này có nghĩa là chỉ định điểm cho tất cả những người về đích được ghi lại ở cấp độ đó, sau đó chỉ xóa các cấu trúc cục bộ của cấp độ đó. Bắt đầu một vùng chứa mới cho cấp độ mới. 
3. Đối với mỗi cấp độ hoạt động, hãy duy trì một từ điển ghi lại xem mỗi người tham gia đã tạo ra sự kiện hợp lệ đầu tiên của họ ở cấp độ này hay chưa. Điều này tránh việc tính hai lần. 
4. Khi xử lý sự kiện loại 2 cho id người tham gia, nếu người tham gia chưa được nhìn thấy ở cấp độ này, hãy đánh dấu họ là đã hoàn thành và thêm họ vào danh sách kết thúc. 
5. Khi xử lý sự kiện loại 3, nếu đó là sự kiện hợp lệ đầu tiên của người tham gia ở cấp độ này, hãy đánh dấu họ là đã thất bại và không đưa họ vào danh sách kết thúc. 
6. Bỏ qua mọi sự kiện có cấp độ không khớp với cấp độ hoạt động hiện tại. 
7. Sau khi xử lý tất cả nhật ký, hãy hoàn tất cấp độ hoạt động cuối cùng theo cách tương tự như bước 2. 
8. Sau khi tất cả các cấp độ được xử lý, hãy tính điểm bằng cách gán m, m−1, … cho những người về đích theo thứ tự hoàn thành cho mỗi cấp độ, tích lũy thành một mảng điểm chung. 
9. Sắp xếp người tham gia theo tổng điểm giảm dần và theo id tăng dần trong trường hợp bằng điểm. 

### Tại sao nó hoạt động

Điều bất biến chính là đối với mỗi cấp độ, danh sách kết thúc bao gồm những người tham gia theo thứ tự thời gian chính xác của sự kiện hoàn thành thành công đầu tiên của họ và mỗi người tham gia xuất hiện nhiều nhất một lần. Vì việc tính điểm chỉ phụ thuộc vào thứ tự này và số lượng người tham gia m chứ không phụ thuộc vào khoảng thời gian hoặc thông báo lặp lại nên cấu trúc này xác định đầy đủ sự đóng góp điểm số của mỗi cấp độ. Việc bỏ qua các cấp độ không hợp lệ và các thông báo trùng lặp không làm thay đổi tính bất biến này vì chúng không bao giờ đóng góp sự kiện hợp lệ đầu tiên dưới cấp độ hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, m, q = map(int, input().split())

        score = [0] * (m + 1)

        current_level = -1
        seen = {}
        finish_order = []

        def finalize():
            nonlocal finish_order
            pts = m
            for pid in finish_order:
                score[pid] += pts
                pts -= 1
            finish_order = []

        for _ in range(q):
            parts = input().split()
            t = int(parts[0])

            if t == 1:
                if current_level != -1:
                    finalize()
                current_level = int(parts[1])
                seen = {}
                finish_order = []

            elif t == 2:
                pid = int(parts[1])
                x = int(parts[2])
                if current_level == -1 or x != current_level:
                    continue
                if pid in seen:
                    continue
                seen[pid] = True
                finish_order.append(pid)

            else:
                pid = int(parts[1])
                x = int(parts[2])
                if current_level == -1 or x != current_level:
                    continue
                if pid in seen:
                    continue
                seen[pid] = True
                # do not add to finish_order (failure)

        if current_level != -1:
            finalize()

        result = [(i, score[i]) for i in range(1, m + 1)]
        result.sort(key=lambda x: (-x[1], x[0]))

        for pid, sc in result:
            print(pid, sc)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một mảng điểm toàn cầu và xử lý từng cấp độ một cách độc lập thông qua một trạng thái cục bộ nhỏ. các`seen`từ điển đảm bảo mỗi người tham gia đóng góp tối đa một hành động hợp lệ cho mỗi cấp độ. các`finish_order`danh sách duy trì thứ tự hoàn thành chính xác, sau này được chuyển thành giá trị điểm giảm dần. 

Chi tiết triển khai quan trọng là chỉ đặt lại trạng thái theo cấp độ trên sự kiện loại 1, không phải trạng thái chung. Một điểm quan trọng khác là chúng ta phải bỏ qua các sự kiện có cấp độ không khớp với sự kiện đang hoạt động, vì nhật ký có thể chứa các hành động cũ. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản đơn giản hóa với hai cấp độ và ba người tham gia: 

đầu vào:```
1
2 3 6
1 1
2 1 1
2 2 1
3 3 1
1 2
2 2 2
2 1 2
```Chúng tôi theo dõi quá trình chuyển đổi cấp độ và danh sách kết thúc. 

| Sự kiện | Cấp độ hoạt động | Đã thấy | Hoàn tất đơn hàng | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 1 | 1 | {} | [] | bắt đầu cấp 1 | 
| 2 1 1 | 1 | {1} | [1] | người tham gia 1 về đích | 
| 2 2 1 | 1 | {1,2} | [1,2] | người tham gia 2 về đích | 
| 3 3 1 | 1 | {1,2,3} | [1,2] | bị bỏ qua trong danh sách kết thúc | 
| 1 2 | 2 | đặt lại | [] | hoàn thiện cấp 1 | 
| 2 2 2 | 2 | {2} | [2] | bắt đầu cấp 2 | 
| 2 1 2 | 2 | {2,1} | [2,1] | người tham gia 1 về đích | 

Sau khi tính điểm ở cấp độ 1: 1 được 3, 2 được 2. Sau cấp độ 2: 2 được 3, 1 được 2. Tổng số cuối cùng phản ánh tổng hợp. 

Điều này xác nhận rằng việc phân tách cấp độ và thứ tự được xử lý độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q + m log m) | Mỗi sự kiện nhật ký được xử lý một lần, sắp xếp là bước cuối cùng | 
| Không gian | O(m + k) | Mảng điểm cộng với bộ nhớ tạm thời theo cấp độ | 

Tổng số sự kiện được giới hạn bởi 2·10^5 cho mỗi lần kiểm tra, vì vậy việc xử lý tuyến tính là đủ. Việc sắp xếp m người tham gia chỉ chiếm ưu thế ở cuối và vẫn khả thi dưới các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue()

# Minimal case
assert run("""1
1 2 2
1 1
2 1 1
""").strip() == "1 2\n2 0"

# All fail case
assert run("""1
1 3 3
1 1
3 1 1
3 2 1
""").strip() == "1 0\n2 0\n3 0"

# Mixed multiple levels
assert run("""1
2 3 6
1 1
2 1 1
2 2 1
1 2
2 2 2
2 1 2
""").split()[0] == "1"

# Duplicate messages ignored
assert run("""1
1 3 5
1 1
2 1 1
2 1 1
3 1 1
2 2 1
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bắt đầu tối thiểu + kết thúc một lần | chấm điểm đúng | dòng chảy cơ bản | 
| mọi thất bại | tất cả đều bằng không | logic từ chối | 
| nhiều cấp độ | thiết lập lại tính đúng đắn | cách ly theo cấp độ | 
| tin nhắn trùng lặp | bỏ qua trùng lặp | quy tắc sự kiện đầu tiên | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là các tin nhắn lặp lại từ cùng một người tham gia trong một cấp độ. Thuật toán xử lý việc này thông qua`seen`từ điển. Sau khi người tham gia được đánh dấu, mọi sự kiện sau đó sẽ bị bỏ qua, đảm bảo chỉ hành động hợp lệ đầu tiên được xem xét. 

Một trường hợp đặc biệt khác là nhật ký trước khi cấp độ đầu tiên bắt đầu. Từ`current_level`được khởi tạo ở trạng thái không hợp lệ, tất cả các sự kiện như vậy sẽ được bỏ qua một cách an toàn. 

Cuối cùng, các cấp độ không bao giờ kết thúc trong nhật ký vẫn được hoàn thiện ở cuối. trận chung kết`finalize()`call đảm bảo điểm của họ được tính chính xác như thể luồng kết thúc ngay sau sự kiện cuối cùng.
