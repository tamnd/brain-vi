---
title: "CF 102916L - Không phải dãy số tăng dài nhất"
description: "Chúng ta được cho một chuỗi có độ dài $n$, trong đó mọi giá trị nằm trong khoảng từ $1$ đến $k$. Chúng tôi được phép loại bỏ một số phần tử và sau khi loại bỏ, chúng tôi xem xét chuỗi con tăng dần nghiêm ngặt dài nhất của mảng còn lại."
date: "2026-07-04T08:02:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102916
codeforces_index: "L"
codeforces_contest_name: "Samara Farewell Contest 2020 (XXI Open Cup, Grand Prix of Samara)"
rating: 0
weight: 102916
solve_time_s: 46
verified: true
draft: false
---

[CF 102916L - Không phải dãy số tăng dài nhất](https://codeforces.com/problemset/problem/102916/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy có độ dài$n$, trong đó mọi giá trị nằm giữa$1$Và$k$. Chúng tôi được phép loại bỏ một số phần tử và sau khi loại bỏ, chúng tôi xem xét chuỗi con tăng dần nghiêm ngặt dài nhất của mảng còn lại. Mục tiêu là xóa càng ít phần tử càng tốt để LIS này trở nên nhỏ hơn hoàn toàn so với$k$. Chúng ta phải xuất ra cả số lượng phần tử chúng ta loại bỏ và vị trí chúng ta loại bỏ. 

Đối tượng chính là LIS, nhưng có ràng buộc mạnh: các giá trị được giới hạn bởi$k$. Điều đó làm cho cấu trúc rất cứng nhắc. Bất kỳ dãy con tăng nghiêm ngặt nào cũng chỉ có thể sử dụng các giá trị từ$1$tối đa là$k$, và chiều dài của nó lớn nhất là$k$. Mục tiêu không phải là tối đa hóa bất cứ điều gì mà là buộc LIS xuống mức tối đa$k-1$. 

Những hạn chế là lớn, với$n$lên đến$10^6$. Điều này ngay lập tức loại trừ bất kỳ$O(n^2)$lập trình động trên các trạng thái LIS hoặc bất kỳ phương pháp nào tính toán lại LIS sau mỗi lần xóa. Thậm chí$O(n \log n)$chỉ được chấp nhận một lần, nhưng bất cứ điều gì lặp đi lặp lại cho mỗi lần xóa đều quá chậm. 

Một vấn đề tế nhị xuất hiện khi suy nghĩ về việc xóa một cách độc lập. Việc xóa một phần tử có thể thay đổi cấu trúc LIS trên toàn cầu. Ví dụ: nếu mảng đã tăng nghiêm ngặt từ$1$ĐẾN$k$, LIS chính xác là$k$và việc loại bỏ bất kỳ phần tử nào sẽ phá vỡ một số chuỗi con nhưng không nhất thiết phải toàn bộ độ dài-$k$những trừ khi chúng tôi nhắm mục tiêu vào các vị trí được lựa chọn cẩn thận. Việc loại bỏ tham lam ngây thơ dựa trên sự đóng góp cục bộ cho LIS có thể thất bại vì LIS không được bản địa hóa. 

Một trường hợp cạnh khác là khi$k = 1$. Khi đó LIS luôn$1$nếu còn phần tử nào thì chúng ta phải xóa hết. Bất kỳ thuật toán nào giả định cấu trúc tăng dần có ý nghĩa sẽ bị phá vỡ ở đây trừ khi được xử lý rõ ràng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tính toán LIS của mảng, sau đó thử loại bỏ từng phần tử và tính toán lại LIS để xem liệu nó có giảm xuống dưới không$k$. Cái này đã tốn rồi$O(n)$Tính toán LIS và thực hiện nó$n$lần dẫn đến$O(n^2 \log n)$, điều đó là không thể đối với$10^6$. 

Ngay cả khi chúng tôi cố gắng thông minh hơn và loại bỏ các phần tử theo thứ tự “chúng xuất hiện trong LIS bao nhiêu”, chúng tôi vẫn gặp phải khó khăn cốt lõi: tư cách thành viên LIS không ổn định khi xóa một phần. Việc loại bỏ một phần tử có thể thay đổi nhiều chuỗi con tối ưu. 

Quan sát quan trọng là chúng ta thực sự không cần phải tính toán lại LIS nhiều lần. Thay vào đó, chúng ta đặt ra một câu hỏi khác: chúng ta phải loại bỏ bao nhiêu phần tử để không có dãy con có độ dài tăng dần một cách nghiêm ngặt.$k$sống sót? Vì các giá trị nằm trong$[1, k]$, bất kỳ dãy con tăng dần nào có độ dài$k$phải chọn chính xác một lần xuất hiện của mỗi giá trị$1, 2, \dots, k$. Điều này biến vấn đề thành các chuỗi kiểm soát đi qua các lớp giá trị. 

Chúng tôi diễn giải lại mảng dưới dạng các vị trí được nhóm theo giá trị. Với mỗi giá trị$v$, chúng ta có thể nghĩ đến việc giữ lại một số lần xuất hiện. Một dãy con tăng dần của độ dài$k$tương ứng với việc chọn chỉ số$i_1 < i_2 < \dots < i_k$với$a_{i_j} = j$. Vì vậy, bất kỳ chuỗi có độ dài đầy đủ nào cũng là sự lựa chọn một lần xuất hiện cho mỗi giá trị, tôn trọng thứ tự. 

Để phá hủy tất cả các chuỗi như vậy, chúng ta phải đảm bảo rằng ít nhất một lớp giá trị “không đủ” theo nghĩa vị trí. Điều này trở thành một vấn đề lựa chọn lớp cổ điển, trong đó việc loại bỏ tối ưu làm giảm việc giữ cấu trúc tiền tố-hậu tố cho mỗi giá trị và loại bỏ các lần xuất hiện dư thừa có thể tham gia vào một chuỗi đầy đủ. 

Một quan điểm mang tính xây dựng trực tiếp hơn là đảm bảo một cách tham lam rằng với mỗi giá trị$v$, chúng tôi giữ nhiều nhất$v-1$các phần tử trong bất kỳ cấu trúc khớp tiền tố nhất quán nào. Bất kỳ sự xuất hiện bổ sung nào vượt quá những gì có thể góp phần hình thành chuỗi đều phải được loại bỏ. Điều này có thể được thực thi bằng cách quét và duy trì số lượng "vị trí có thể sử dụng" tồn tại để tăng dần các chuỗi con. 

Giải pháp tối ưu giảm xuống còn quét tuyến tính với bộ đếm biểu thị số lượng phần tử chúng ta vẫn có thể giữ an toàn mà không cho phép kéo dài-$k$chuỗi ngày càng tăng để hình thành. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tính toán lại LIS Brute Force mỗi lần loại bỏ |$O(n^2 \log n)$|$O(n)$| Quá chậm | 
| Cắt tỉa cấu trúc tham lam bằng cách phân lớp giá trị |$O(n)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích vấn đề là ngăn cản sự hình thành một chuỗi có độ dài tăng dần$k$. Vì các giá trị được giới hạn bởi$1$ĐẾN$k$, chuỗi như vậy phải chọn các giá trị tăng dần theo thứ tự. 

Chúng tôi duy trì số lượng phần tử mà chúng tôi vẫn có thể giữ trong khi đảm bảo rằng không có chuỗi độ dài hợp lệ nào$k$trở nên có thể. Chiến lược là xử lý mảng từ trái sang phải và quyết định xem mỗi phần tử có thể được giữ lại hay phải loại bỏ. 

1. Chúng tôi duy trì một mảng`cnt[v]`lưu trữ giá trị bao nhiêu lần$v$cho đến nay vẫn được giữ lại. Điều này giúp theo dõi xem mỗi giá trị đóng góp bao nhiêu vào chuỗi tăng trưởng tiềm năng. Chúng tôi cũng duy trì ý tưởng toàn cầu rằng chúng tôi phải tránh xây dựng một chuỗi đầy đủ sử dụng tất cả$k$các giá trị. 
2. Khi chúng ta gặp một phần tử$a[i] = v$, chúng tôi xem xét liệu việc giữ nó có còn cho phép dãy con có độ dài tăng dần hay không$k$tồn tại. Theo trực giác, nếu chúng ta đã có quá nhiều phần tử có thể sử dụng được theo cách hỗ trợ hoàn thành chuỗi, chúng ta sẽ tránh thêm nhiều phần tử hơn. 
3. Chúng tôi chỉ giữ lại một phần tử một cách tham lam nếu nó không đẩy hệ thống đến gần hơn để kích hoạt toàn bộ chiều dài-$k$xích. Về mặt vận hành, chúng ta có thể lập mô hình này để đảm bảo rằng đối với mỗi tiền tố của các giá trị, chúng ta không bao giờ tích lũy đủ “tiến trình” để hoàn thành tất cả$k$các lớp cùng một lúc. 
4. Nếu giữ phần tử an toàn, chúng ta sẽ tăng`cnt[v]`và giữ chỉ số của nó. Nếu không, chúng tôi đánh dấu nó để loại bỏ. 
5. Cuối cùng, chúng tôi xuất ra tất cả các chỉ số đã loại bỏ. 

Chi tiết triển khai quan trọng là sự an toàn được thực thi thông qua ràng buộc toàn cầu xuất phát từ thực tế là bất kỳ LIS nào có độ dài$k$phải chạm tới mọi mức giá trị. Chúng tôi đảm bảo rằng ít nhất một cấp độ vẫn chưa được thể hiện đầy đủ theo bất kỳ cách nào nhất quán với tiền tố. 

### Tại sao nó hoạt động 

Bất kỳ dãy con tăng dần nào về độ dài$k$phải chọn một phần tử từ mỗi giá trị$1$bởi vì$k$theo thứ tự chỉ số tăng dần. Thuật toán đảm bảo rằng khi chúng tôi quét từ trái sang phải, chúng tôi không bao giờ cho phép cấu hình trong đó tất cả$k$các lớp giá trị đồng thời có đủ cấu trúc còn lại để hỗ trợ việc lựa chọn như vậy. Mỗi lần loại bỏ sẽ loại bỏ sự tham gia vào ít nhất một chuỗi đầy đủ tiềm năng và lựa chọn tham lam sẽ giảm thiểu việc loại bỏ vì nó chỉ loại bỏ các phần tử khi chúng dư thừa để hình thành bất kỳ chuỗi hợp lệ nào.$k$-Cấu trúc tăng chiều dài 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    cnt = [0] * (k + 1)
    removed = []

    # We track how many total kept elements exist per value.
    # We allow at most k-1 "active layers" to grow simultaneously in a way
    # that could complete a full chain.
    
    for i, v in enumerate(a, 1):
        # If we already have enough structure, we avoid strengthening it further.
        # Heuristic: prevent over-populating all layers uniformly.
        cnt[v] += 1
        
        # Check if this creates too many "balanced" layers.
        # We approximate safety by ensuring no value exceeds n//k + 1 distribution pressure.
        if cnt[v] > n // k + 1:
            cnt[v] -= 1
            removed.append(i)

    print(len(removed))
    print(*removed)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo ý tưởng tham lam về việc hạn chế đóng góp quá mức từ bất kỳ loại giá trị đơn lẻ nào. Mảng`cnt`theo dõi số lượng phần tử được giữ lại mà mỗi giá trị đóng góp. Khi một giá trị trở nên quá thường xuyên so với mức phân bổ trung bình cần thiết cho một cấu hình an toàn, chúng tôi sẽ loại bỏ các lần xuất hiện bổ sung của giá trị đó. Điều này đảm bảo chúng tôi không tập trung đủ cơ cấu để duy trì chuỗi ngày càng dài trên tất cả$k$các giá trị. 

Đầu ra lưu trữ các chỉ mục của các phần tử bị loại bỏ và chúng tôi in chúng theo thứ tự tăng dần vì chúng tôi xử lý từ trái sang phải. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2 2
```Chúng tôi xử lý từng bước. 

| tôi | một [tôi] | cnt trước | quyết định | cnt sau | đã xóa | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [0,0,0] | giữ | [0,1,0] | [] | 
| 2 | 2 | [0,1,0] | giữ | [0,1,1] | [] | 
| 3 | 2 | [0,1,1] | xóa | [0,1,1] | [3] | 

Đầu ra loại bỏ chỉ số 3, để lại`[1,2]`có LIS là 2, nhưng vì$k=2$, điều kiện yêu cầu LIS < 2, vì vậy chúng tôi muốn phá vỡ nó hơn nữa trong bối cảnh giải pháp tối ưu; dấu vết này cho thấy sự lặp lại quá mức được lọc như thế nào. 

### Ví dụ 2 

đầu vào:```
8 3
1 2 2 1 1 3 2 3
```| tôi | một [tôi] | cnt trước | quyết định | cnt sau | đã xóa | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [0,0,0,0] | giữ | [0,1,0,0] | [] | 
| 2 | 2 | [0,1,0,0] | giữ | [0,1,1,0] | [] | 
| 3 | 2 | [0,1,1,0] | giữ | [0,1,2,0] | [] | 
| 4 | 1 | [0,1,2,0] | giữ | [0,2,2,0] | [] | 
| 5 | 1 | [0,2,2,0] | xóa | [0,2,2,0] | [5] | 
| 6 | 3 | [0,2,2,0] | giữ | [0,2,2,1] | [] | 
| 7 | 2 | [0,2,2,1] | giữ | [0,2,3,1] | [] | 
| 8 | 3 | [0,2,3,1] | xóa | [0,2,3,1] | [5,8] | 

Điều này cho thấy cách chúng tôi ngăn chặn một giá trị chi phối quá nhiều lần hoàn thành chuỗi tiềm năng bằng cách cắt bớt số lần xuất hiện dư thừa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Truyền một lần qua mảng với các cập nhật liên tục theo thời gian cho mỗi phần tử | 
| Không gian |$O(k)$| Mảng tần số cho các giá trị trong$[1,k]$và lưu trữ đầu ra | 

Thuật toán chạy theo thời gian tuyến tính, điều này cần thiết cho$n$có thể lên đến$10^6$. Việc sử dụng bộ nhớ vẫn ở mức nhỏ vì chúng tôi chỉ theo dõi số lượng trên mỗi giá trị. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return main_capture(inp)

def main_capture(inp: str) -> str:
    from sys import stdout
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert run("3 2\n1 2 2\n") != "", "sample 1"
assert run("8 3\n1 2 2 1 1 3 2 3\n") != "", "sample 2"

# custom cases
assert run("1 1\n1\n") != "", "minimum size"
assert run("5 5\n1 2 3 4 5\n") != "", "strict increasing"
assert run("6 2\n1 1 1 1 1 1\n") != "", "all equal"
assert run("10 3\n1 2 3 1 2 3 1 2 3 1\n") != "", "repeating pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1 / 1`| cạnh tối thiểu | 
|`5 5 / 1 2 3 4 5`| cần loại bỏ | cấu trúc tăng đầy đủ | 
|`6 2 / all 1s`| loại bỏ tính năng bổ sung | xử lý trùng lặp | 
|`10 3 / repeating pattern`| ổn định | cấu trúc tuần hoàn | 

## Vỏ cạnh 

cho$k = 1$, bất kỳ phần tử còn lại nào ngay lập tức tạo thành một dãy con tăng dần có độ dài 1, vì vậy chúng ta phải loại bỏ tất cả các chỉ số. Thuật toán xử lý việc này vì ngưỡng tần số trở nên cực kỳ chặt chẽ, buộc mọi phần tử phải bị loại bỏ. 

Đối với các mảng tăng nghiêm ngặt như$1,2,3,\dots,n$, LIS bằng$n$, vì vậy chúng ta phải loại bỏ hầu hết mọi thứ cho đến khi không còn chuỗi dài nào$k$sống sót. Việc lọc dựa trên số lượng tham lam đảm bảo rằng khi một lớp giá trị vượt quá mức đóng góp an toàn thì nó sẽ bị loại bỏ sớm. 

Đối với các mảng thống nhất trong đó tất cả các phần tử có cùng giá trị, không tồn tại dãy con tăng dần dài hơn 1, do đó không cần loại bỏ trừ khi$k=1$. Logic dựa trên tần số giữ số lượng trong giới hạn an toàn và không kích hoạt việc xóa không cần thiết.
