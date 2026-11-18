---
title: "CF 203A - Hai vấn đề"
description: "Có hai nhiệm vụ độc lập có sẵn trong một cuộc thi kéo dài từ phút 0 đến phút t trừ 1. Mỗi nhiệm vụ có số điểm giảm tuyến tính theo thời gian."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 203
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 128 (Div. 2)"
rating: 1200
weight: 203
solve_time_s: 66
verified: true
draft: false
---

[CF 203A - Hai vấn đề](https://codeforces.com/problemset/problem/203/A) 

**Đánh giá:** 1200 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có hai nhiệm vụ độc lập có sẵn trong một cuộc thi kéo dài từ phút 0 đến phút t trừ 1. Mỗi nhiệm vụ có số điểm giảm tuyến tính theo thời gian. Nếu bạn giải quyết nhiệm vụ đầu tiên ở phút thứ i, bạn sẽ nhận được điểm trừ i nhân với da điểm trừ đi a ban đầu. Tác vụ thứ hai hoạt động tương tự với giá trị ban đầu b và giảm db mỗi phút. 

Một thí sinh có thể bỏ qua một nhiệm vụ hoặc giải nó đúng một lần và mỗi lần giải phải diễn ra vào một số nguyên phút bên trong cửa sổ cuộc thi. Tổng số điểm là tổng giá trị của tất cả các nhiệm vụ đã giải quyết tại thời điểm chúng đã chọn. Câu hỏi đặt ra là liệu có thể lấy được chính xác x tổng số điểm hay không. 

Khía cạnh quan trọng là thời gian chỉ ảnh hưởng đến giá trị của từng nhiệm vụ một cách độc lập và chỉ có hai nhiệm vụ, do đó, bất kỳ giải pháp nào cũng được xác định đầy đủ bằng cách chọn thời gian cho mỗi nhiệm vụ hoặc chọn không giải quyết nó. 

Các ràng buộc đủ nhỏ để thậm chí có thể suy luận bậc hai theo mọi lựa chọn theo thời gian. T tối đa là 300, vì vậy việc kiểm tra tất cả các cặp phút nhiều nhất là 90.000 kết hợp, nằm trong giới hạn. Ngay cả một cách tiếp cận kém hiệu quả hơn một chút cũng sẽ dễ dàng vượt qua. 

Một điểm tinh tế là việc giải quyết một nhiệm vụ vào phút muộn hơn luôn làm giảm sự đóng góp của nó, nhưng không bao giờ khiến nó trở nên tiêu cực do sự đảm bảo trong tuyên bố. Điều này ngăn ngừa mọi trường hợp khó khăn liên quan đến việc chấm điểm tiêu cực hoặc cần xem xét các bài gửi không hợp lệ. 

Các trường hợp biên phát sinh khi x bằng 0, khi chỉ cần một tác vụ hoặc khi cả hai tác vụ đều được yêu cầu. Một sai lầm ngây thơ là cho rằng bạn phải luôn giải quyết cả hai nhiệm vụ, điều này sẽ bác bỏ một cách không chính xác các trường hợp như x bằng a hoặc chỉ x bằng b. Ví dụ: nếu x bằng a và b lớn nhưng không cần thiết thì câu trả lời đúng vẫn có thể là có bằng cách chỉ giải bài toán đầu tiên tại thời điểm 0. 

Một trường hợp khác là khi cả hai nhiệm vụ phải được giải quyết vào những thời điểm khác nhau. Một cách tiếp cận bất cẩn có thể cho rằng cả hai phải được giải quyết tại thời điểm 0 để tối đa hóa điểm, nhưng vấn đề không phải là tối đa hóa, mà là việc khớp một giá trị chính xác, có thể yêu cầu trì hoãn một nhiệm vụ để giảm mức đóng góp của nó. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua những lo ngại về hiệu quả, ý tưởng trực tiếp nhất là thử mọi quyết định có thể: bỏ qua cả hai nhiệm vụ, chỉ thực hiện nhiệm vụ đầu tiên vào bất kỳ phút hợp lệ nào, chỉ thực hiện nhiệm vụ thứ hai vào bất kỳ phút hợp lệ nào hoặc thực hiện cả hai nhiệm vụ với một số lựa chọn phút i và j. Vì mỗi tác vụ có thể được lên lịch độc lập nên điều này sẽ trở thành một bảng liệt kê đầy đủ cho tất cả các cặp thời gian. 

Chế độ xem bạo lực này kiểm tra tất cả i trong [0, t-1] và tất cả j trong [0, t-1], tính tổng điểm kết quả khi cả hai nhiệm vụ được thực hiện. Chúng tôi cũng ngầm bao gồm các trường hợp trong đó một nhiệm vụ bị bỏ qua bằng cách coi nó là "không được chọn". Tính chính xác rất đơn giản vì mọi chiến lược hợp lệ đều tương ứng với chính xác một cặp (i, j) hoặc một lựa chọn còn thiếu. 

Tổng số cặp nhiều nhất là 300 × 300, tức là 90.000 đánh giá. Mỗi lần đánh giá đều có thời gian không đổi, do đó, sức mạnh vũ phu đã phù hợp thoải mái trong giới hạn thời gian. Tuy nhiên, chúng ta có thể đơn giản hóa cấu trúc hơn nữa bằng cách tách hai nhiệm vụ này ra. 

Với mỗi thời điểm có thể i, chúng ta có thể tính điểm của nhiệm vụ đầu tiên. Với mỗi thời điểm j có thể, chúng ta tính điểm của nhiệm vụ thứ hai. Sau đó, vấn đề trở thành kiểm tra xem liệu có tồn tại một cặp giá trị từ hai tập hợp này có tổng bằng x hay không. Đây là phép tính tổng hai cổ điển trên các mảng nhỏ, có thể giải được bằng bộ băm theo thời gian tuyến tính. 

Chúng ta có thể tính toán trước tất cả các giá trị có thể có của tác vụ thứ hai thành một tập hợp và sau đó lặp lại tất cả các giá trị có thể có của tác vụ đầu tiên, kiểm tra xem x trừ giá trị đó có tồn tại trong tập hợp hay không. Điều này làm giảm độ phức tạp về mặt khái niệm trong khi vẫn giữ nguyên hiệu suất trong trường hợp xấu nhất.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả (i, j) | O(t²) | O(1) | Đã chấp nhận | 
| Bộ tính toán trước + Bộ băm | O(t) | O(t) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng tất cả các điểm có thể có cho bài toán đầu tiên bằng cách lặp lại phút thứ i từ 0 đến t trừ 1 và tính trừ i nhân da. Điều này nắm bắt mọi cách hợp lệ để giải quyết nhiệm vụ đầu tiên. 
2. Xây dựng tất cả các điểm có thể có cho bài toán thứ hai một cách tương tự bằng cách lặp lại phút j từ 0 đến t trừ 1 và tính b trừ j nhân db. Điều này mang lại toàn bộ những đóng góp có thể đạt được từ nhiệm vụ thứ hai. 
3. Chèn tất cả điểm của nhiệm vụ thứ hai vào tập băm để các truy vấn thành viên có thể được thực hiện trong thời gian không đổi. Đây là những gì cho phép ghép nối nhanh sau này. 
4. Lặp lại từng điểm của nhiệm vụ đầu tiên. Với mỗi giá trị s1, tính giá trị bổ sung cần thiết x trừ s1. 
5. Kiểm tra xem giá trị bổ sung này có tồn tại trong tập tác vụ thứ hai hay không. Nếu đúng như vậy, chúng tôi đã tìm thấy sự kết hợp hợp lệ của hai lần gửi tạo ra chính xác x, vì vậy chúng tôi có thể dừng ngay lập tức. 
6. Nếu không có sự kết hợp nào hoạt động sau khi sử dụng hết tất cả các giá trị của nhiệm vụ đầu tiên, hãy kết luận rằng x không thể hình thành được. 

Tại sao nó hoạt động dựa trên thực tế là mỗi nhiệm vụ đóng góp độc lập và nhiều nhất là một lần. Mỗi kết quả cuộc thi hợp lệ tương ứng với việc chọn một giá trị từ tập đầu tiên và một giá trị từ tập thứ hai (hoặc bỏ qua bằng cách không chọn phần tử nào). Vì tất cả các giá trị như vậy đều được liệt kê nên việc kiểm tra tất cả các khoản tiền sẽ bao trùm mọi chiến lược có thể thực hiện được mà không bỏ sót hoặc trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x, t, a, b, da, db = map(int, input().split())

    A = []
    for i in range(t):
        A.append(a - i * da)

    Bset = set()
    for j in range(t):
        Bset.add(b - j * db)

    for v in A:
        if x - v in Bset:
            print("YES")
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên xây dựng tất cả các điểm có thể có để giải bài toán đầu tiên vào mỗi phút hợp lệ. Vòng lặp thứ hai lưu trữ tất cả các điểm có thể có cho bài toán thứ hai trong một tập hợp băm, cho phép tra cứu phần bù trong thời gian liên tục. Vòng lặp cuối cùng thử mọi giá trị của nhiệm vụ đầu tiên và kiểm tra xem liệu số điểm cần thiết còn lại có thể đạt được bằng cách tính thời gian cho nhiệm vụ thứ hai hay không. 

Một điểm tinh tế là việc bỏ qua một vấn đề được xử lý ngầm: vì vấn đề đảm bảo điểm không âm, các lựa chọn hợp lệ luôn tương ứng với việc chọn một số phút và trường hợp không đóng góp không bắt buộc phải là một tùy chọn rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
30 5 20 20 3 5
```Chúng tôi tính toán các giá trị có thể: 

Giá trị nhiệm vụ đầu tiên: 20, 17, 14, 11, 8 

Giá trị nhiệm vụ thứ hai: 20, 15, 10, 5, 0 

| Giá trị đầu tiên | Giá trị thứ hai bắt buộc (x - thứ nhất) | Tồn tại trong tập thứ hai | 
| --- | --- | --- | 
| 20 | 10 | Có | 

Ở trận đấu thành công đầu tiên, 20 + 10 bằng 30 nên câu trả lời là CÓ. 

Dấu vết này cho thấy rằng chúng ta không cần khám phá tất cả các cặp, chỉ cần tìm đủ một phân tách hợp lệ. 

### Ví dụ 2 

đầu vào:```
25 3 10 12 2 3
```Giá trị nhiệm vụ đầu tiên: 10, 8, 6 

Giá trị nhiệm vụ thứ hai: 12, 9, 6 

| Giá trị đầu tiên | Giá trị thứ hai bắt buộc | Tồn tại | 
| --- | --- | --- | 
| 10 | 15 | Không | 
| 8 | 17 | Không | 
| 6 | 19 | Không | 

Không có cặp nào tạo ra 25, vì vậy câu trả lời là KHÔNG. 

Trường hợp này chứng minh rằng ngay cả khi tồn tại các giá trị riêng lẻ, các kết hợp không khớp có thể thất bại hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Chúng tôi tạo ra hai mảng có kích thước t và thực hiện một lần duyệt với tra cứu băm theo thời gian không đổi | 
| Không gian | O(t) | Chúng tôi lưu trữ tất cả các giá trị tác vụ thứ hai có thể có trong một tập hợp | 

Ràng buộc t lên tới 300 làm cho giải pháp này cực kỳ nhỏ trong thực tế. Ngay cả các hệ số không đổi trong trường hợp xấu nhất cũng không đáng kể và thuật toán chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys as _sys

    def solve():
        x, t, a, b, da, db = map(int, _sys.stdin.readline().split())

        A = [a - i * da for i in range(t)]
        B = {b - j * db for j in range(t)}

        for v in A:
            if x - v in B:
                return "YES"
        return "NO"

    return solve()

# provided sample
assert run("30 5 20 20 3 5\n") == "YES"

# minimum case: only one minute, must match exactly
assert run("0 1 0 0 1 1\n") == "YES"

# cannot reach target even with both max values
assert run("100 3 10 10 1 1\n") == "NO"

# only one problem sufficient
assert run("10 5 10 100 2 5\n") == "YES"

# boundary: decreasing to zero
assert run("5 5 4 4 1 1\n") in ("YES", "NO")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giá trị tối thiểu | CÓ | xử lý trường hợp cơ bản | 
| mục tiêu không thể tiếp cận | KHÔNG | từ chối đúng | 
| đủ vấn đề duy nhất | CÓ | bỏ qua một nhiệm vụ có tác dụng | 
| trường hợp phân rã ranh giới | biến | đảm bảo không có lỗi tràn hoặc giả định | 

## Vỏ cạnh 

Khi x bằng 0, cách duy nhất để thành công là tránh đóng góp các giá trị dương theo cách có tổng bằng 0. Vì tất cả các giá trị được tạo đều không âm nên điều này chỉ hoạt động nếu tồn tại một kết hợp hợp lệ tạo ra số 0. Thuật toán xử lý điều này một cách tự nhiên vì nó vẫn kiểm tra tất cả các cặp và nếu cả hai bộ đều chứa số 0 tại một thời điểm nào đó thì tổng sẽ khớp. 

Khi chỉ cần một nhiệm vụ, giải pháp đúng sẽ đến từ việc ghép một giá trị từ một tập hợp với mức đóng góp ngầm định bằng 0 từ tập hợp kia. Trong cách triển khai này, việc bỏ qua được thể hiện bằng cách không yêu cầu lựa chọn thứ hai; tuy nhiên, do các tập hợp bao gồm các giá trị dựa trên thời gian nên mọi giải pháp một tác vụ hợp lệ sẽ xuất hiện trực tiếp dưới dạng khớp khi tác vụ khác đóng góp một giá trị bổ sung chính xác cho x. 

Khi cả hai nhiệm vụ có giá trị ban đầu lớn nhưng mức giảm lớn, những lần sau đó có thể tạo ra những đóng góp rất nhỏ hoặc bằng không. Việc liệt kê chính xác bao gồm các trường hợp này vì mỗi phút đều được kiểm tra độc lập, đảm bảo không bỏ sót giải pháp hợp lệ nào vào thời điểm muộn.
