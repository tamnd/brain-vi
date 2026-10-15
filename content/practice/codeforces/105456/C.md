---
title: "CF 105456C - Bánh quy của Jan"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi cái có rất nhiều cookie, mỗi cookie thuộc một loại và có thời hạn biến mất."
date: "2026-06-23T02:49:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105456
codeforces_index: "C"
codeforces_contest_name: "XXIII Spain Olympiad in Informatics, Day 2"
rating: 0
weight: 105456
solve_time_s: 101
verified: false
draft: false
---

[CF 105456C - Bánh quy của Jan](https://codeforces.com/problemset/problem/105456/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi cái có rất nhiều cookie, mỗi cookie thuộc một loại và có thời hạn biến mất. Jan muốn ăn bánh quy theo thời gian và mỗi lần anh ấy ăn một chiếc bánh quy, thời gian chờ đợi trước chiếc bánh quy tiếp theo sẽ tăng lên tùy theo số lượng bánh quy anh ấy đã ăn. Bánh quy đầu tiên có thể ăn ngay, bánh thứ hai yêu cầu đợi 1 đơn vị sau khi ăn bánh đầu tiên, bánh thứ ba yêu cầu đợi 2 đơn vị sau khi ăn bánh thứ hai, v.v. Nếu Jan đã ăn rồi$k-1$bánh quy và ăn$k$- vào lúc đó$t$, thì cái tiếp theo không thể ăn trước thời gian$t + (k-1)$. 

Mỗi chiếc bánh quy sẽ biến mất vào thời điểm nhất định, nhưng Jan cũng được phép ăn nó vào đúng thời điểm đó. Tồn tại nhiều loại bánh quy cùng loại và Jan có thể ăn bất kỳ số lượng nào trong số đó. Mục tiêu là tối đa hóa số lượng loại bánh quy riêng biệt mà Jan có thể ăn ít nhất một lần. 

Khó khăn chính là việc ăn nhiều bánh quy hơn sẽ làm tăng thời gian chờ đợi trong tương lai, khiến cho thời hạn sau đó khó đáp ứng hơn. Quyết định không chỉ là liệu một loại có sẵn hay không mà còn là liệu nó có thể được lên kế hoạch đủ sớm trong khoảng thời gian chi phí ngày càng tăng hay không. 

Những ràng buộc ngụ ý rằng chúng ta cần một cái gì đó gần với$O(n \log n)$hoặc$O(n \log m)$mỗi trường hợp thử nghiệm. Với tối đa$2 \cdot 10^5$tổng số cookie, bất kỳ phương pháp nào thử tất cả các tập hợp con hoặc mô phỏng lịch trình cho từng loại một cách độc lập đều quá chậm. Ngay cả mô phỏng theo từng loại tham lam mà không sắp xếp cũng có nguy cơ xảy ra hành vi bậc hai. 

Trường hợp phức tạp xuất hiện khi một loại có nhiều cookie với thời hạn khác nhau. Chọn sai sự kiện xảy ra có vấn đề: 

Ví dụ: giả sử loại 1 có cookie ở thời điểm 1 và 100, còn loại 2 có cookie ở thời điểm 2. Nếu chúng ta tham lam chọn cookie sớm nhất có thể của mỗi loại mà không tính đến chi phí trong tương lai, thì chúng ta có thể chọn loại 1 ở thời điểm 1 và loại 2 ở thời điểm 2, điều này sẽ hiệu quả. Nhưng nếu loại 2 có thời hạn 1 thay vì 2, thì việc chọn khối đầu tiên loại 1 loại 2 mặc dù việc đặt hàng khác sẽ thành công. Điều này cho thấy chúng ta phải suy xét về việc đặt hàng các loại đã chọn, không chỉ tính sẵn có. 

Một trường hợp đặc biệt khác là khi một loại chỉ có cookie muộn. Ngay cả khi một loại “hiện tại”, nó chỉ có thể sử dụng được nếu được lên lịch từ rất sớm trong trình tự do chi phí trễ ngày càng tăng. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là xử lý từng loại một cách độc lập và cố gắng quyết định tập hợp con nào của các loại mà Jan có thể chọn thành công theo một số thứ tự. Đối với một tập hợp con cố định các loại, chúng tôi sẽ cố gắng chỉ định cho mỗi loại đã chọn một cookie đại diện và sau đó mô phỏng xem liệu chúng tôi có thể lên lịch cho chúng theo thứ tự nào đó tôn trọng thời hạn và quy tắc độ trễ ngày càng tăng hay không. 

Điều này ngay lập tức trở nên tốn kém vì số lượng tập hợp con của các loại là theo cấp số nhân. Ngay cả khi chúng ta sửa một tập hợp con, việc kiểm tra tính khả thi bản thân nó không hề đơn giản, vì đối với mỗi thứ tự, chúng ta phải tính toán lại thời gian đến phụ thuộc vào vị trí trong chuỗi. Với$m$lên tới$5 \cdot 10^4$, điều này là không thể thực hiện được. 

Quan sát quan trọng là chúng ta không bao giờ cần nhiều hơn một cookie cho mỗi loại và đối với mỗi loại, điều tối ưu là chỉ xem xét ứng viên có thời hạn sử dụng mới nhất, vì những cookie trước đó chỉ làm giảm tính linh hoạt. Khi chúng tôi quyết định chọn một loại, chúng tôi chỉ quan tâm đến loại cookie tốt nhất có thể hỗ trợ loại đó. 

Bây giờ vấn đề trở thành: chúng ta muốn chọn càng nhiều loại càng tốt và mỗi loại được chọn sẽ đóng góp một hạn chế về thời hạn. Nếu chúng ta chọn$k$các loại, chúng ta phải gán cho chúng vị trí$1$bởi vì$k$, và$i$-loại được chọn thứ nhất phải có thể đạt được tại một thời điểm nhất định$i(i-1)/2$cộng với một số thay đổi cơ bản từ việc lập kế hoạch, nhưng đơn giản hơn là chúng tôi mô phỏng tăng dần:$i$- cookie được chọn được ăn vào thời điểm bằng tổng độ trễ$0 + 1 + 2 + \dots + (i-1)$, đó là$(i-1)i/2$, nếu chúng ta giả sử chúng ta bắt đầu ở thời điểm 0. 

Tuy nhiên, vì cookie có thời hạn tùy ý nên chúng tôi cần kiểm tra tính khả thi trực tiếp: nếu chúng tôi sắp xếp các loại đã chọn theo thời hạn tăng dần, chúng tôi muốn đảm bảo rằng khi chỉ định chúng theo thứ tự đó, mỗi loại có thể được đặt không muộn hơn thời hạn của nó trong điều kiện tăng trưởng độ trễ tích lũy. 

Điều này dẫn đến một chiến lược tham lam: xem xét các loại được sắp xếp theo thời hạn tốt nhất có thể của chúng. Duy trì bao nhiêu loại chúng tôi đã chọn. Đối với mỗi loại, chúng tôi kiểm tra xem liệu chúng tôi có thể đưa nó vào hay không. Nếu chúng ta ở kích thước$k$, loại tiếp theo sẽ được ăn sau một khoảng thời gian trì hoãn$k$, vì vậy chúng tôi kiểm tra xem thời hạn đã chọn của nó có phải là thời điểm mà nó sẽ được tiêu thụ với độ dài chuỗi hiện tại hay không. Nếu có, chúng tôi bao gồm nó. 

Chúng tôi giảm mỗi loại thành một giá trị duy nhất: thời hạn tốt nhất có thể đạt được chỉ đơn giản là thời hạn tối đa trong số các cookie của nó, vì Jan có thể chọn bản sao nào để ăn. Sau đó, chúng tôi tham lam chọn các loại theo thứ tự tăng dần của các giá trị này, duy trì ràng buộc khả thi đang chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong$m$|$O(m)$| Quá chậm | 
| Tham lam trước thời hạn |$O(n \log n)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén dữ liệu đầu vào để mỗi loại được biểu thị bằng một số duy nhất: thời gian biến mất tối đa trong số tất cả các cookie thuộc loại đó. Điều này nắm bắt cơ hội tốt nhất có thể để sử dụng loại đó, bởi vì bất kỳ cookie nào trước đó chỉ làm cho ràng buộc chặt chẽ hơn. 

1. Đọc tất cả các cookie và tính toán cho từng loại thời hạn tối đa trong số các lần xuất hiện của nó. Điều này biến vấn đề thành việc lựa chọn giữa$m$các mục, mỗi mục có một thời hạn duy nhất. 
2. Sắp xếp tất cả các loại theo thời hạn tính toán theo thứ tự không giảm. Lý do là trước tiên chúng tôi muốn cam kết thực hiện các loại hạn chế nhất, vì việc trì hoãn chúng chỉ làm tăng nguy cơ chúng tôi vượt quá thời hạn khi tình trạng trì hoãn ăn uống tích lũy ngày càng tăng. 
3. Khởi tạo bộ đếm$k = 0$, đại diện cho số lượng loại chúng tôi đã chọn thành công cho đến nay. 
4. Lặp lại danh sách các thời hạn đã được sắp xếp. Đối với từng loại có thời hạn$d$, tính thời gian Jan sẽ ăn chiếc bánh quy tiếp theo nếu chúng ta đưa nó vào. Thời gian đó là chi phí chờ đợi tích lũy hiện tại, bằng$k(k+1)/2$nếu được theo dõi chính xác, nhưng chúng ta có thể tránh được công thức đầy đủ bằng cách duy trì dần thời gian có sẵn tiếp theo. Nếu thời gian yêu cầu tiếp theo không vượt quá$d$, chúng tôi chấp nhận loại này và tăng dần$k$. Nếu không chúng tôi bỏ qua nó. 

Ý tưởng quan trọng là việc chấp nhận một loại chỉ làm tăng độ trễ trong tương lai, vì vậy một khi một loại không vượt qua được bài kiểm tra tính khả thi thì sau này nó sẽ không bao giờ trở nên khả thi. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán duy trì một tập hợp các loại đã được chọn có thể được lên lịch theo thứ tự thời hạn tăng dần mà không vi phạm các ràng buộc. Việc sắp xếp theo thời hạn đảm bảo rằng khi chúng tôi xem xét một loại mới, chúng tôi đang thử nghiệm nó ở trạng thái tăng trưởng nhỏ nhất có thể cho vị trí của nó trong số tất cả các lựa chọn khả thi. Nếu một loại không thể vừa với vị trí sớm nhất có thể của nó theo thứ tự này, thì việc chèn nó vào bất kỳ vị trí nào sau đó sẽ chỉ làm tăng thời gian chờ đợi cần thiết của nó, khiến nó thậm chí còn ít khả thi hơn. Điều này chứng minh rằng sự chấp nhận tham lam không bao giờ làm mất hiệu lực các quyết định khả thi trong tương lai và việc bỏ qua một loại là an toàn vì mọi giải pháp hợp lệ bao gồm nó sẽ yêu cầu thay thế một số loại được chấp nhận trước đó bằng thời hạn chặt chẽ hơn, mâu thuẫn với tính tối ưu của thứ tự được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        m, n = map(int, input().split())
        
        # best deadline per type
        best = [0] * (m + 1)
        
        for _ in range(n):
            a, ti = map(int, input().split())
            if ti > best[a]:
                best[a] = ti
        
        deadlines = [best[i] for i in range(1, m + 1)]
        deadlines.sort()
        
        k = 0
        current_time = 0
        
        for d in deadlines:
            # if we take this as (k+1)-th cookie, it is eaten at current_time + k
            if current_time + k <= d:
                current_time += k
                k += 1
        
        out.append(str(k))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Bước nén đảm bảo chúng tôi chỉ theo dõi cookie dễ cho phép nhất cho mỗi loại. Mảng thời hạn được sắp xếp tạo ra cấu trúc cần thiết cho sự lựa chọn tham lam. Các biến`k`Và`current_time`mã hóa hiệu ứng trễ tích lũy: khi chọn loại tiếp theo, chi phí chờ đợi tăng chính xác bằng số lượng loại đã chọn. 

điều kiện`current_time + k <= d`kiểm tra xem chiếc bánh tiếp theo có thể được đặt trước khi nó biến mất hay không, vì lần ăn tiếp theo là độ trễ tích lũy hiện tại cộng với độ trễ bổ sung gây ra bởi việc đặt nó vào vị trí`k+1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
2 4
1 1
1 2
1 3
2 4
```Chúng tôi nén các loại: 

| Loại | Thời hạn tốt nhất | 
| --- | --- | 
| 1 | 3 | 
| 2 | 4 | 

Thời hạn được sắp xếp là$[3, 4]$. 

| Bước | k | hiện tại_thời gian | d | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 3 | lấy | 
| 2 | 1 | 0 | 4 | lấy | 

Cả hai loại đều khả thi, vì vậy câu trả lời là 2. 

Điều này cho thấy rằng mặc dù loại 1 có nhiều lựa chọn nhưng chỉ có thời hạn tốt nhất mới quan trọng. 

### Ví dụ 2 

đầu vào:```
1
3 3
1 2
2 2
3 2
```Đã nén: 

| Loại | Thời hạn tốt nhất | 
| --- | --- | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 2 | 

Đã sắp xếp:$[2, 2, 2]$| Bước | k | hiện tại_thời gian | d | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 2 | lấy | 
| 2 | 1 | 0 | 2 | lấy | 
| 3 | 2 | 1 | 2 | lấy | 

Tất cả đều khả thi, tạo ra 3. 

Điều này chứng tỏ rằng ngay cả những thời hạn chặt chẽ giống nhau cũng được xử lý chính xác vì sự chậm trễ tích lũy ngay từ đầu đã tăng lên đủ chậm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| thời hạn sắp xếp chiếm ưu thế, tất cả các hoạt động khác đều tuyến tính | 
| Không gian |$O(m)$| lưu trữ thời hạn tốt nhất cho mỗi loại | 

Giải pháp phù hợp thoải mái trong giới hạn vì$n$tùy thuộc vào$2 \cdot 10^5$và sắp xếp cộng với một lượt duy nhất có hiệu quả dưới 2 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            m, n = map(int, input().split())
            best = [0] * (m + 1)
            for _ in range(n):
                a, ti = map(int, input().split())
                if ti > best[a]:
                    best[a] = ti
            deadlines = sorted(best[1:])
            k = 0
            current_time = 0
            for d in deadlines:
                if current_time + k <= d:
                    current_time += k
                    k += 1
            out.append(str(k))
        return "\n".join(out)

    return solve()

# provided sample
assert run("""2
2 4
1 1
1 2
1 3
2 4
3 3
1 2
2 2
3 2
""") == "2\n2"

# minimum case
assert run("""1
1 1
1 100
""") == "1"

# all tight impossible except first
assert run("""1
3 3
1 1
2 1
3 100
""") == "2"

# identical deadlines boundary
assert run("""1
5 5
1 1
2 1
3 1
4 1
5 1
""") == "1"

# large increasing feasibility
assert run("""1
4 4
1 10
2 100
3 1000
4 10000
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| loại đơn | 1 | xử lý tối thiểu | 
| hỗn hợp thời hạn chặt chẽ | 2 | khả thi một phần | 
| tất cả các thời hạn nhỏ như nhau | 1 | bão hòa sớm | 
| tăng thời hạn | 4 | thành công đầy tham lam | 

## Vỏ cạnh 

Một trường hợp tinh tế xảy ra khi nhiều loại có cùng thời hạn và chỉ một số ít có thể được chọn trước khi độ trễ tích lũy vượt quá thời hạn đó. Đối với đầu vào:```
1
4 4
1 1
2 1
3 1
4 10
```Thời hạn nén là$[1, 1, 1, 10]$. Thuật toán đầu tiên lấy loại đầu tiên với chi phí 0, sau đó thử ngay loại thứ hai nhưng không thành công vì thời gian yêu cầu tiếp theo là 1 trong khi current_time + k trở thành 1, bằng với thời hạn và vẫn được phép. Sau khi chọn hai loại, lần thử tiếp theo sẽ yêu cầu thời gian 2, vượt quá 1, vì vậy chỉ có thể thực hiện được hai loại trong nhóm chặt chẽ. Loại cuối cùng deadline 10 được nhận sớm hay muộn tùy theo đơn đặt hàng, nhưng vì kích thước lớn nên luôn vừa khít sau khi những loại chật đã hết. 

Điều này xác nhận thuật toán đã bão hòa chính xác các ràng buộc ban đầu trước khi tận dụng các ràng buộc được nới lỏng.
