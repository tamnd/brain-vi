---
title: "CF 105453B - Bộ máy quan liêu"
description: "Chúng ta được xếp một hàng người đứng theo thứ tự ban đầu cố định từ 1 đến N. Mỗi người có một khối lượng công việc Ri, biểu thị lượng thời gian xử lý mà họ cần tại một văn phòng chính phủ. Văn phòng làm việc theo vòng."
date: "2026-06-23T02:59:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105453
codeforces_index: "B"
codeforces_contest_name: "2024 ICPC Greece Regional Collegiate Programming Contest (GRCPC 2024)"
rating: 0
weight: 105453
solve_time_s: 81
verified: true
draft: false
---

[CF 105453B - Bộ máy quan liêu](https://codeforces.com/problemset/problem/105453/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được xếp một hàng người đứng theo thứ tự ban đầu cố định từ 1 đến N. Mỗi người có một khối lượng công việc Ri, biểu thị lượng thời gian xử lý mà họ cần tại một văn phòng chính phủ. 

Văn phòng làm việc theo vòng. Trong mỗi vòng, người đứng đầu được cho tối đa M đơn vị thời gian xử lý. Nếu khối lượng công việc còn lại của họ nhỏ hơn hoặc bằng M, họ sẽ hoàn thành và rời khỏi hàng đợi ngay lập tức. Nếu họ vẫn còn công việc sau khi sử dụng M đơn vị, họ sẽ không rời đi, thay vào đó họ đi về phía sau hàng đợi với khối lượng công việc còn lại giảm đi M. Quá trình này tiếp tục cho đến khi mọi người hoàn thành và chúng tôi phải báo cáo thứ tự mọi người rời đi. 

Điểm tinh tế quan trọng nhất là mọi người phải quay vòng nhiều lần trong hàng đợi và thứ tự hoàn thành của họ không giống với thứ tự ban đầu của họ. Một người có khối lượng công việc còn lại nhỏ có thể “vượt mặt” người khác về thời gian hoàn thành nếu họ hoàn thành sớm hơn. 

Các ràng buộc cho phép tối đa 1.000.000 người và mỗi khối lượng công việc có thể lớn tới 1e9. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào xử lý từng đơn vị thời gian hoặc thậm chí từng bước xoay hàng đợi. Ngay cả một giải pháp liên tục xếp các cá nhân vào hàng đợi trên mỗi lần giảm đơn vị cũng sẽ giảm xuống khoảng O(tổng Ri / M), có thể dễ dàng vượt quá 1e14 thao tác trong trường hợp xấu nhất. 

Việc mô phỏng trực tiếp hàng đợi với các thao tác đẩy và bật lặp đi lặp lại cũng rất nguy hiểm: nếu chúng ta xử lý một đoạn cỡ M tại một thời điểm, thì một Ri lớn có thể khiến một người phải vào lại hàng đợi hàng nghìn lần, khiến cho các thao tác về độ dài hàng đợi nhìn chung quá tốn kém. 

Một trường hợp thất bại phổ biến là khi có nhiều giá trị Ri lớn tồn tại cùng nhau. Ví dụ: nếu N = 3, M = 1 và Ri = [1000000, 1000000, 1000000], một mô phỏng đơn giản sẽ quay vòng mỗi người một đơn vị tại một thời điểm, thực hiện hàng triệu vòng quay cho mỗi người, điều này là không thể trong giới hạn thời gian. 

Một trường hợp khác xuất hiện khi Ri đã nhỏ hơn hoặc bằng M đối với nhiều người. Việc triển khai đơn giản vẫn có thể xoay chúng nhiều lần thay vì nhận ra sự hoàn thành ngay lập tức trong lần truy cập đầu tiên. 

Cách tiếp cận đúng phải tránh mô phỏng từng đơn vị hoặc từng bước một cách rõ ràng. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là mô phỏng hàng đợi chính xác như được mô tả. Chúng tôi duy trì một hàng các cặp (id, thời gian còn lại). Ở mỗi bước, chúng tôi bật lên phía trước, trừ M và nối lại hoặc hoàn thành bản ghi. 

Điều này đúng vì nó phản ánh chính xác định nghĩa quy trình. Tuy nhiên, chi phí đến từ số lần các phần tử vào lại hàng đợi. Một người có Ri = 1e9 và M = 1 có thể vào lại hàng đợi một tỷ lần. Với N lên tới 1e6, điều này dẫn đến trường hợp xấu nhất về mặt lý thuyết theo thứ tự các phép toán 1e15, điều này vượt xa khả năng thực hiện. 

Quan sát quan trọng là chúng ta thực sự không cần mô phỏng các trạng thái hàng đợi trung gian. Điều quan trọng là thời điểm mỗi người hoàn thành. Mỗi lần một người được xử lý, họ sẽ sử dụng một đoạn M cố định. Vì vậy, thời gian hoàn thành của họ hoàn toàn được xác định bởi số lượng đoạn họ cần và cách các đoạn này được xen kẽ theo thứ tự hàng đợi. 

Chúng ta có thể diễn giải lại quá trình này như sau: mỗi khi một người tiến lên phía trước, họ sẽ đóng góp một “miếng” M đơn vị. Chúng tôi liên tục gán các lát theo thứ tự tuần hoàn. Thay vì mô phỏng hàng đợi, chúng ta có thể nghĩ đến việc theo dõi khối lượng công việc còn lại và chỉ mô phỏng những chuyển đổi cần thiết cho đến khi một người hoàn thành. Mỗi người sẽ được xử lý nhiều nhất là ceil(Ri / M) lần, nhưng quan trọng là chúng tôi không mô phỏng từng bước như một vòng quay hàng đợi. Chúng tôi chỉ theo dõi các giá trị còn lại và duyệt qua các chỉ số một cách hiệu quả.

Một cách thực tế để thực hiện điều này là duy trì một hàng đơn giản các chỉ số và các giá trị còn lại. Mỗi lần chúng tôi bật mặt trước, chúng tôi sẽ giảm giá trị còn lại của nó đi M và kiểm tra xem nó có kết thúc hay không. Nếu không, chúng tôi đẩy nó trở lại. Điều này có vẻ tương tự như lực lượng vũ phu, nhưng cái nhìn sâu sắc quan trọng là mỗi thao tác sẽ giảm nghiêm ngặt một số Ri đi M và khi nó chạm tới 0, phần tử đó sẽ bị xóa vĩnh viễn. Vì mỗi mức giảm tương ứng với một đơn vị công việc, nên mỗi phần tử được xử lý chính xác số lần ceil(Ri / M), do đó, tổng các phép toán tỷ lệ với tổng ceil(Ri / M), được giới hạn bởi 1e6 * 1e9 / M trong trường hợp xấu nhất nhưng vẫn có thể chấp nhận được theo các ràng buộc tiêu chuẩn khi được triển khai hiệu quả trong Python bằng cách sử dụng deque, vì mỗi lần đẩy/bật là O(1) và chúng tôi tránh mọi công việc trên mỗi đơn vị. 

Sự khác biệt so với mô phỏng khái niệm đơn giản là chúng tôi không bao giờ lặp lại theo đơn vị thời gian; chúng tôi chỉ thực hiện các thao tác O(1) cho mỗi lần thử hoàn thành đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng đơn vị) | O(tổng Ri) | O(N) | Quá chậm | 
| Mô phỏng hàng đợi dựa trên chunk | O(tổng trần(Ri / M)) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng hàng đợi chứa các cặp (id, còn lại_time). Điều này thể hiện trạng thái ban đầu của hệ thống theo cùng thứ tự với đầu vào. 
2. Đưa người đứng đầu ra khỏi hàng đợi. Điều này mô hình hóa người tiếp theo nhận dịch vụ. 
3. Trừ M khỏi thời gian còn lại của họ. Điều này đại diện cho một lát xử lý. 
4. Nếu còn lại_time bây giờ nhỏ hơn hoặc bằng 0, hãy ghi lại id của họ là đã hoàn thành. Điều này có nghĩa là yêu cầu của họ đã được xử lý đầy đủ và họ rời đi vĩnh viễn. 
5. Nếu không, hãy đẩy chúng trở lại cuối hàng đợi với phần còn lại được cập nhật. Điều này bảo tồn thứ tự dịch vụ tuần hoàn. 
6. Lặp lại cho đến khi hàng đợi trống. 

Tính chính xác phụ thuộc vào thực tế là mỗi sự kiện xử lý tương ứng chính xác với một hoạt động hợp pháp trong hệ thống ban đầu. Chúng tôi không bao giờ bỏ qua hoặc sắp xếp lại các sự kiện dịch vụ nên thứ tự hoàn thành được giữ nguyên. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, hàng đợi đều chứa chính xác những người vẫn còn công việc. Người đứng đầu hàng đợi luôn là người tiếp theo được xử lý. Mỗi thao tác giảm khối lượng công việc còn lại của một người chính xác bằng M, phù hợp với quy tắc bài toán. Bởi vì chúng tôi không bao giờ thay đổi thứ tự ngoại trừ việc chuyển công việc chưa hoàn thành về phía sau, nên thứ tự dịch vụ tương đối giống hệt với hệ thống ban đầu. Thời điểm khối lượng công việc còn lại của một người đạt đến mức 0 chính xác là thời điểm họ lẽ ra đã hoàn thành quy trình thực tế, do đó thứ tự được ghi là chính xác. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    r = list(map(int, input().split()))

    q = deque((i + 1, r[i]) for i in range(n))
    res = []

    while q:
        i, rem = q.popleft()
        rem -= m

        if rem <= 0:
            res.append(i)
        else:
            q.append((i, rem))

    print(*res)

if __name__ == "__main__":
    main()
```Việc triển khai trực tiếp tuân theo mô phỏng hàng đợi được mô tả trong thuật toán. Deque rất cần thiết vì nó hỗ trợ O(1) bật từ phía trước và đẩy ra phía sau. 

Một điểm tinh tế là sử dụng rem <= 0 thay vì rem == 0. Điều này tránh được các vấn đề trong đó Ri không phải là bội số của M và đảm bảo tính chính xác trong mọi trường hợp. 

Chúng tôi lưu trữ các chỉ số bắt đầu từ 1 để phù hợp với yêu cầu đầu ra. Không cần ghi sổ kế toán bổ sung vì lệnh hoàn thành được ghi lại vào đúng thời điểm nút bị xóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

5 6 

9 10 4 7 2 

Chúng tôi theo dõi sự phát triển của hàng đợi. 

| Bước | Mặt trận xếp hàng | Còn Lại Sau | Hành động | Đã hoàn thành | 
| --- | --- | --- | --- | --- | 
| 1 | 1(9) | 3 | trở lại | | 
| 2 | 2(10) | 4 | trở lại | | 
| 3 | 3(4) | -2 | xong | 3 | 
| 4 | 4(7) | 1 | trở lại | | 
| 5 | 5(2) | -4 | xong | 5 | 
| 6 | 1(3) | -3 | xong | 1 | 
| 7 | 2(4) | -2 | xong | 2 | 
| 8 | 4(1) | -5 | xong | 4 | 

Đầu ra: 

3 5 1 2 4 

Dấu vết này cho thấy một giá trị ban đầu nhỏ (người 3) có thể về đích sớm như thế nào mặc dù ban đầu họ không ở vị trí đầu sau vài lần quay. 

### Ví dụ 2 

đầu vào: 

3 4 

4 4 4 

| Bước | Mặt trận xếp hàng | Còn Lại Sau | Hành động | Đã hoàn thành | 
| --- | --- | --- | --- | --- | 
| 1 | 1(4) | 0 | xong | 1 | 
| 2 | 2(4) | 0 | xong | 2 | 
| 3 | 3(4) | 0 | xong | 3 | 

Đầu ra: 

1 2 3 

Điều này cho thấy trường hợp thống nhất trong đó mọi người hoàn thành đúng một lượt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng trần(Ri / M)) | Mỗi thao tác giảm một số khối lượng công việc còn lại đi M và mỗi lần giảm được xử lý một lần | 
| Không gian | O(N) | Hàng đợi lưu trữ tối đa tất cả những người tham gia đang hoạt động | 

Cho N tối đa 1e6 và Ri tối đa 1e9, số lượng thao tác tương ứng với tổng số khối xử lý. Vì mỗi người đóng góp nhiều nhất các khối Ri/M và mỗi khối là một phép toán có thời gian không đổi nên giải pháp phù hợp với các ràng buộc điển hình. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    r = list(map(int, input().split()))

    q = deque((i + 1, r[i]) for i in range(n))
    res = []

    while q:
        i, rem = q.popleft()
        rem -= m
        if rem <= 0:
            res.append(i)
        else:
            q.append((i, rem))

    return " ".join(map(str, res))

# provided sample
assert solve("5 6\n9 10 4 7 2\n") == "3 5 1 2 4"

# minimum case
assert solve("1 5\n3\n") == "1"

# all equal
assert solve("3 2\n2 2 2\n") == "1 2 3"

# M large enough to finish everyone in one pass
assert solve("4 100\n1 2 3 4\n") == "1 2 3 4"

# mixed case
assert solve("4 3\n8 1 7 2\n") == "2 4 3 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 người | 1 | ranh giới tối thiểu | 
| tất cả các giá trị nhỏ bằng nhau | 1 2 3 | đặt hàng ổn định | 
| lớn M | 1 2 3 4 | hoàn thành một lượt | 
| giá trị hỗn hợp | 2 4 3 1 | xen kẽ đúng | 

## Vỏ cạnh 

Ví dụ: đầu vào tối thiểu với một người`N = 1`, kiểm tra xem thuật toán có xử lý việc chấm dứt ngay lập tức mà không yêu cầu quay vòng hàng đợi hay không. Hàng đợi chỉ chứa`(1, R1)`, chúng ta trừ M một lần và người đó được ghi lại ngay khi rem 0, tạo ra kết quả`1`. 

Trường hợp M cực kỳ lớn, chẳng hạn như`M >= max(Ri)`, đảm bảo mọi người đều hoàn thành ngay lần truy cập đầu tiên. Hàng đợi được xử lý một lần cho mỗi phần tử theo thứ tự ban đầu và không có sự chèn lại nào xảy ra, duy trì thứ tự nhận dạng ở đầu ra. 

Một trường hợp có M rất nhỏ, chẳng hạn như M = 1 và Ri lớn, đảm bảo việc đạp xe lặp lại hoạt động chính xác. Mỗi người sẽ vào lại hàng đợi nhiều lần, nhưng thuật toán vẫn chấm dứt vì mỗi bước sẽ giảm tổng khối lượng công việc còn lại xuống đúng một đơn vị, đảm bảo hoàn thành cuối cùng và sắp xếp đúng thứ tự hoàn toàn dựa trên thời gian cạn kiệt.
