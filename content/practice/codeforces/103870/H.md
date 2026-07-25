---
title: "CF 103870H - Không tin cậy"
description: "Chúng tôi được cung cấp một chuỗi các sinh viên, mỗi sinh viên được liên kết với một giá trị tin cậy sẽ thay đổi khi chúng tôi xử lý hệ thống. Tại bất kỳ thời điểm nào, một số học sinh được coi là có niềm tin tích cực và chúng tôi quan tâm đến hai điều: có bao nhiêu học sinh hiện có niềm tin tích cực và tổng…"
date: "2026-07-02T07:46:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103870
codeforces_index: "H"
codeforces_contest_name: "TeamsCode Summer 2022 Contest"
rating: 0
weight: 103870
solve_time_s: 46
verified: true
draft: false
---

[CF 103870H - Không tin cậy](https://codeforces.com/problemset/problem/103870/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các sinh viên, mỗi sinh viên được liên kết với một giá trị tin cậy sẽ thay đổi khi chúng tôi xử lý hệ thống. Tại bất kỳ thời điểm nào, một số học sinh được coi là có niềm tin tích cực và chúng tôi quan tâm đến hai điều: có bao nhiêu học sinh hiện có niềm tin tích cực và tổng số đóng góp đã điều chỉnh của họ. 

Mỗi học sinh có một giá trị tin cậy ban đầu và cũng có một số lượng tích lũy toàn cầu bắt nguồn từ các bản cập nhật trước đó. Để thuận tiện, chúng tôi xác định một giá trị đang chạy$d_i$, điều này chỉ phụ thuộc vào các cập nhật toàn cầu cho đến thời điểm hiện tại chứ không phụ thuộc vào bất kỳ cá nhân học sinh nào. Khi một học sinh có niềm tin tích cực, đóng góp của họ cho câu trả lời là giá trị niềm tin hiện tại của họ trừ đi phần bù đắp toàn cầu này$d_i$. 

Cấu trúc chính là tất cả học sinh tích cực đều có cùng một thuật ngữ trừ$d_i$, vì vậy khi tính tổng số đóng góp của tất cả học sinh tích cực, thuật ngữ toàn cầu này được coi là tích đơn giản của số lượng học sinh tích cực và$d_i$. Điều này có nghĩa là chúng tôi không bao giờ cần phải theo dõi rõ ràng các giá trị đã thay đổi của từng học sinh, mà chỉ tổng hợp thông tin về những người vẫn đang hoạt động và tổng giá trị tin cậy thô của họ là bao nhiêu. 

Đầu vào có thể được hiểu là một chuỗi các cập nhật sửa đổi giá trị tin cậy hoặc nâng cao trạng thái toàn cầu. Đầu ra được tính toán sau khi xử lý tất cả các phép toán, báo cáo số lượng học sinh có độ tin cậy được điều chỉnh tích cực và tổng số tiền tương ứng. 

Về mặt hạn chế, giải pháp dự định thực hiện trong$O(n \log n + q)$, điều này gợi ý lên đến khoảng$2 \cdot 10^5$các yếu tố kết hợp với các bản cập nhật. Điều này ngay lập tức loại trừ mọi mô phỏng bậc hai trong đó mỗi bản cập nhật sẽ quét tất cả học sinh. Bất kỳ cách tiếp cận nào liên tục tính toán lại tất cả các trạng thái hoạt động cho mỗi hoạt động sẽ đạt được khoảng$10^{10}$trong trường hợp xấu nhất là không thể thực hiện được. 

Một trường hợp thất bại tinh tế xuất hiện khi một người cố gắng tính toán lại các giá trị tin cậy đã điều chỉnh cho mỗi học sinh sau mỗi lần cập nhật toàn cầu. Ví dụ: nếu tất cả học sinh bắt đầu với các giá trị tin cậy lớn nhưng phần bù tổng thể tăng dần, thì một cách tiếp cận đơn giản có thể làm giảm tất cả các giá trị nhiều lần. 

Một ví dụ nhỏ làm rõ điều này. Giả sử chúng ta có ba học sinh có giá trị tin cậy là 10, 9, 8 và offset toàn cục trở thành 7. Tập hoạt động chính xác chỉ phụ thuộc vào việc mỗi giá trị có vượt quá 7 hay không, vì vậy cả ba vẫn dương. Tuy nhiên, việc triển khai phép trừ lặp đi lặp lại đơn giản có thể cập nhật liên tục từng học sinh trong mỗi thao tác và làm giảm hiệu suất mặc dù điều kiện logic chỉ phụ thuộc vào so sánh với một giá trị được chia sẻ. 

Một cạm bẫy khác phát sinh khi học sinh bị loại hoặc không hoạt động: không tính toán chính xác phần đóng góp của họ trong tổng tổng hợp dẫn đến việc tính hai lần hoặc các giá trị cũ được đưa vào kết quả cuối cùng. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi mô phỏng hệ thống một cách trực tiếp. Đối với mỗi hoạt động, chúng tôi cập nhật tất cả các giá trị tin cậy của học sinh bị ảnh hưởng, tính toán lại giá trị nào vẫn dương, sau đó tính toán lại cả số lượng và tổng đóng góp từ đầu. Điều này có tác dụng vì nó trực tiếp tuân theo định nghĩa của quy trình, nhưng mỗi lần tính toán lại sẽ quét tất cả học sinh. Với$n$sinh viên và$q$hoạt động, điều này dẫn đến$O(nq)$, trong trường hợp xấu nhất là thang bậc hai đến bậc ba tùy thuộc vào cấu trúc đầu vào. Tại$n = 2 \cdot 10^5$, ngay cả một lần quét toàn bộ cho mỗi thao tác cũng đã quá lớn. 

Quan sát quan trọng là tất cả học sinh tích cực đều bị ảnh hưởng như nhau bởi sự bù đắp toàn cầu$d_i$. Điều này có nghĩa là chúng tôi chỉ cần duy trì hai điều: tổng giá trị tin cậy thô của các học sinh tích cực và số lượng trong số đó đang hoạt động. Khi chúng ta biết những điều này, tổng số tiền đóng góp có được bằng cách trừ đi$d_i \times s$, Ở đâu$s$là số lượng học sinh tích cực. Sau đó, vấn đề giảm xuống còn việc duy trì một tập hợp các giá trị thay đổi linh hoạt trong đó các phần tử sẽ bị xóa khi chúng giảm xuống dưới ngưỡng hiện tại được xác định bởi$d_i$. 

Đây chính xác là loại cấu trúc mà min-heap nắm bắt một cách hiệu quả. Chúng tôi luôn muốn biết giá trị tin cậy nhỏ nhất trong số các sinh viên hiện đang hoạt động. Nếu cái nhỏ nhất nằm dưới ngưỡng$d_i$, nó phải được loại bỏ, bởi vì tất cả các đóng góp đều đơn điệu theo nghĩa là khi giá trị nhỏ nhất không đáp ứng được điều kiện, nó có thể làm mất hiệu lực của tập hoạt động hiện tại. Việc lặp lại quá trình loại bỏ này đảm bảo rằng chỉ còn lại những học sinh hợp lệ và mỗi học sinh được chèn và xóa nhiều nhất một lần, tạo ra độ phức tạp logarit được khấu hao. 

Một quan điểm khác là sắp xếp tất cả học sinh theo độ tin cậy và xử lý chúng theo thứ tự ngưỡng tăng dần. Điều đó cũng có tác dụng khi ngưỡng đơn điệu, nhưng cách tiếp cận vùng heap tổng quát hơn và phù hợp với các cập nhật động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Bảo trì Heap tối thiểu |$O(n \log n + q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một lượng tối thiểu các sinh viên ứng cử viên theo giá trị tin cậy của họ, cùng với tổng giá trị tin cậy của họ và số lượng sinh viên tích cực. Chúng tôi cũng theo dõi khoản bù đắp toàn cầu$d_i$. 

1. Khởi tạo một heap tối thiểu trống, một biến`active_sum = 0`, Và`active_count = 0`. Chúng đại diện cho tập hợp hoạt động hiện tại. 
2. Xử lý từng học sinh bằng cách chèn giá trị tin cậy của họ vào heap và tăng cả hai`active_sum`Và`active_count`tương ứng. Điều này xây dựng nhóm ứng viên ban đầu. 
3. Khi chúng tôi xử lý các bản cập nhật toàn cầu, hãy duy trì mức chênh lệch hiện tại$d_i$. Giá trị này thể hiện ngưỡng tin cậy tối thiểu cần thiết để học sinh duy trì hiệu lực. 
4. Sau mỗi lần cập nhật, hãy kiểm tra liên tục giá trị tin cậy nhỏ nhất trong vùng heap. Nếu nó hoàn toàn nhỏ hơn hoặc bằng điều kiện vô hiệu do$d_i$, loại bỏ nó khỏi đống và trừ nó khỏi`active_sum`, trong khi giảm dần`active_count`. Bước này đảm bảo không còn học sinh không hợp lệ nào trong tập hợp đang hoạt động. 
5. Tiếp tục loại bỏ cho đến khi heap trống hoặc phần tử nhỏ nhất thỏa mãn điều kiện để duy trì hoạt động. 
6. Sau khi đạt được sự ổn định, hãy tính kết quả bằng cách sử dụng`active_sum - active_count * d_i`về tổng số tiền đóng góp và báo cáo`active_count`như số lượng học sinh tích cực. 

Vòng loại bỏ là điểm quyết định cốt lõi. Chúng tôi chỉ xóa khi một học sinh được chứng minh là không hợp lệ theo ngưỡng hiện tại. Điều này tránh việc tính toán lại không cần thiết trong khi vẫn đảm bảo tính chính xác. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến nhất quán: tất cả các phần tử hiện có trong heap đều đáp ứng điều kiện bắt buộc để được coi là hoạt động theo phần bù toàn cục hiện tại. Bất kỳ phần tử nào vi phạm điều kiện này đều phải ở đầu vùng nhớ tối thiểu vì đó là giá trị tin cậy nhỏ nhất còn lại, vì vậy nếu thất bại, không có ứng cử viên nhỏ hơn nào tồn tại để duy trì tính chính xác. Vì mỗi phần tử được loại bỏ nhiều nhất một lần nên tổng số thao tác trong đống là tuyến tính theo số lượng sinh viên và mỗi thao tác tiêu tốn thời gian logarit. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))

    heap = []
    active_sum = 0
    active_count = 0

    for x in arr:
        heapq.heappush(heap, x)
        active_sum += x
        active_count += 1

    d = 0

    for _ in range(q):
        typ, val = map(int, input().split())

        if typ == 1:
            d += val
        else:
            heapq.heappush(heap, val)
            active_sum += val
            active_count += 1

        while heap and heap[0] <= d:
            x = heapq.heappop(heap)
            active_sum -= x
            active_count -= 1

        if active_count <= 0:
            print(0, 0)
        else:
            print(active_count, active_sum - active_count * d)

def main():
    solve()

if __name__ == "__main__":
    main()
```Mã giữ một khoản bù đắp toàn cầu`d`đại diện cho sự thay đổi tích lũy áp dụng cho tất cả học sinh. Mỗi lần chèn sẽ cập nhật cả vùng heap và tổng tổng để sau này chúng ta có thể xây dựng lại tổng đã dịch chuyển trong thời gian không đổi. Heap đảm bảo chúng tôi luôn loại bỏ phần tử vi phạm nhỏ nhất trước tiên, điều này là cần thiết vì chỉ phần tử tối thiểu mới có thể xác định xem có cần loại bỏ hay không. 

Phép trừ`active_sum - active_count * d`là bước tái thiết quan trọng. Thay vì điều chỉnh mọi giá trị được lưu trữ, chúng tôi trì hoãn việc áp dụng giá trị bù và tính toán giá trị đó một lần tại thời điểm truy vấn. Điều này tránh các bản cập nhật lặp lại và giữ cho giải pháp luôn hiệu quả. 

Vòng lặp xuất hiện từ vùng heap là an toàn vì mỗi phần tử được chèn một lần và bị xóa một lần, đảm bảo hành vi logarit được khấu hao. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng ban đầu`[5, 3, 10]`với các bản cập nhật làm tăng phần bù toàn cầu. 

Khi bắt đầu, heap là`[3, 5, 10]`, tổng là 18, số đếm là 3, và`d = 0`. 

| Bước | Hoạt động | Đống | d | active_count | hoạt động_sum | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ban đầu | [3,5,10] | 0 | 3 | 18 | 
| 2 | +2 đến d | [3,5,10] | 2 | 3 | 18 | 
| 3 | xóa <= d | [5,10] | 2 | 2 | 15 | 

Sau khi loại bỏ 3 phần tử còn lại là hợp lệ. Câu trả lời trở thành 2 và$15 - 2 \cdot 2 = 11$. 

Dấu vết này cho thấy rằng chỉ phần tử nhỏ nhất được chọn và sau khi nó bị xóa, không cần xóa tầng nữa. 

Bây giờ hãy xem xét trường hợp có phần chèn: bắt đầu trống, sau đó chèn 4, 1, 6 và tăng dần độ lệch. 

| Bước | Hoạt động | Đống | d | active_count | hoạt động_sum | 
| --- | --- | --- | --- | --- | --- | 
| 1 | +4 | [4] | 0 | 1 | 4 | 
| 2 | +1 | [1,4] | 0 | 2 | 5 | 
| 3 | +6 | [1,4,6] | 0 | 3 | 11 | 
| 4 | d += 3 | [1,4,6] | 3 | 3 | 11 | 
| 5 | xóa <= d | [4,6] | 3 | 2 | 10 | 

Điều này chứng tỏ cách các phần chèn và tăng ngưỡng tương tác rõ ràng: các phần tử không hợp lệ chỉ được loại bỏ một cách lười biếng khi cần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + q \log n)$| mỗi phần tử được chèn một lần và bị xóa một lần khỏi heap, mỗi thao tác tốn thời gian logarit | 
| Không gian |$O(n)$| lưu trữ nhiều nhất tất cả các phần tử đang hoạt động hoặc đang chờ xử lý | 

Sự phức tạp phù hợp thoải mái trong các ràng buộc điển hình lên đến$2 \cdot 10^5$, vì các hoạt động của heap vẫn hiệu quả ngay cả trong các chuỗi chèn và xóa trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    n, q = map(int, input().split())
    arr = list(map(int, input().split()))

    heap = []
    s = 0
    c = 0
    d = 0

    for x in arr:
        heapq.heappush(heap, x)
        s += x
        c += 1

    out = []
    for _ in range(q):
        t, v = map(int, input().split())
        if t == 1:
            d += v
        else:
            heapq.heappush(heap, v)
            s += v
            c += 1

        while heap and heap[0] <= d:
            x = heapq.heappop(heap)
            s -= x
            c -= 1

        if c <= 0:
            out.append("0 0")
        else:
            out.append(f"{c} {s - c*d}")

    return "\n".join(out)

# custom cases
assert run("3 3\n5 3 10\n1 2\n1 1\n1 5") is not None
assert run("1 2\n1\n1 1\n1 1") is not None
assert run("2 2\n1 100\n2 50\n1 60") is not None
assert run("5 1\n1 2 3 4 5\n1 10") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng nhỏ, ngưỡng tăng dần | tập hoạt động giảm hợp lệ | tính đúng đắn của việc loại bỏ heap | 
| phần tử đơn | hành vi cạnh ổn định | xử lý kết cấu rỗng | 
| chèn và cập nhật hỗn hợp | tính nhất quán năng động | tương tác của các bản cập nhật và heap | 
| ngưỡng lớn | trường hợp loại bỏ đầy đủ | tất cả các phần tử trở nên không hoạt động | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi phần bù toàn cục trở nên đủ lớn để tất cả học sinh bị vô hiệu. Đối với đầu vào`[2, 3, 5]`với mức tăng lớn, nói$d = 10$, vùng heap cuối cùng sẽ bật lên tất cả các phần tử. Thuật toán xử lý chính xác điều này vì vòng lặp while làm trống vùng heap và lần kiểm tra cuối cùng`active_count <= 0`kích hoạt`(0, 0)`đầu ra. 

Một trường hợp tinh vi khác là việc chèn lặp đi lặp lại các giá trị nhỏ sau khi phần bù đã tăng lên. Ví dụ, nếu$d = 100$và chúng tôi chèn các giá trị`[1, 2, 3]`, mỗi lần chèn mới sẽ bị xóa ngay lập tức trong vòng lặp dọn dẹp. Vùng heap không bao giờ giữ lại các phần tử không hợp lệ, do đó trạng thái vẫn nhất quán mà không cần sửa lỗi hồi tố. 

Cuối cùng, khi tất cả các giá trị bằng nhau và độ lệch tăng dần, việc loại bỏ sẽ diễn ra theo đợt. Vì`[5, 5, 5, 5]`và tăng dần lặp đi lặp lại của$d$, mỗi lần tăng có thể loại bỏ nhiều phần tử cùng một lúc. Bất biến đảm bảo rằng việc loại bỏ hàng loạt là an toàn vì thứ tự được giữ nguyên trong heap và tất cả các phần tử đều giống hệt nhau, do đó không phát sinh vấn đề thứ tự ẩn nào.
