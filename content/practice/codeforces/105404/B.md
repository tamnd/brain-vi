---
title: "CF 105404B - Chia"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm cung cấp một danh sách các số nguyên dương. Nhiệm vụ là xác định xem có tồn tại ít nhất một phần tử trong danh sách có thể chia mọi phần tử trong cùng một danh sách hay không, kể cả chính nó một cách tầm thường."
date: "2026-06-23T04:48:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105404
codeforces_index: "B"
codeforces_contest_name: "XXIV Spain Olympiad in Informatics, Online Qualifier 2"
rating: 0
weight: 105404
solve_time_s: 80
verified: false
draft: false
---

[CF 105404B - Phép chia](https://codeforces.com/problemset/problem/105404/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm cung cấp một danh sách các số nguyên dương. Nhiệm vụ là xác định xem có tồn tại ít nhất một phần tử trong danh sách có thể chia mọi phần tử trong cùng một danh sách hay không, kể cả chính nó một cách tầm thường. 

Nói một cách cụ thể hơn, đối với mỗi mảng, chúng ta đang tìm kiếm một chỉ mục$i$sao cho với mọi$j$, giá trị$a_i$chia rẽ$a_j$không có phần còn lại. Nếu phần tử đó tồn tại, chúng tôi trả lời "SI", nếu không chúng tôi trả lời "KHÔNG". 

Kích thước đầu vào buộc chúng ta phải suy nghĩ cẩn thận về hiệu quả của mỗi trường hợp kiểm thử. Có thể có tối đa 100 trường hợp thử nghiệm và mỗi trường hợp thử nghiệm có thể chứa tối đa 10.000 số. Một cách tiếp cận đơn giản để kiểm tra các cặp chia hết cho từng phần tử ứng cử viên so với tất cả các phần tử khác sẽ yêu cầu kiểm tra tới$n^2$mối quan hệ trên mỗi trường hợp thử nghiệm, trong trường hợp xấu nhất đạt đến$10^8$kiểm tra tính chia hết. Đó vẫn là giới hạn nhưng chỉ có thể chuyển sang Python được tối ưu hóa nếu được triển khai cẩn thận, mặc dù điều đó là không cần thiết. 

Một ràng buộc tinh vi hơn đến từ phạm vi giá trị: mỗi số có tối đa$10^9$. Điều này loại trừ bất kỳ phương pháp lập chỉ mục trực tiếp hoặc mảng tần số nào. Cấu trúc hoàn toàn là số học. 

Một trường hợp thường phá vỡ lý luận ngây thơ là các giá trị lặp lại. Ví dụ: trong một mảng như`[4, 2, 4, 6, 8, 4]`, giá trị 4 xuất hiện nhiều lần và là một ứng cử viên hợp lệ, mặc dù nó không chia 2 hoặc 6 ở tất cả các vị trí trừ khi chúng ta kiểm tra cẩn thận tất cả các phần tử. Một trường hợp khác là khi số nhỏ nhất không phải là câu trả lời. Ví dụ`[2, 3, 6]`có 2 làm ứng cử viên nhưng không thành công vì 3 không chia hết cho 2. 

Chế độ thất bại tinh vi thứ hai là giả sử phần tử tối thiểu luôn hoạt động. Vì`[2, 3, 4]`, nhỏ nhất là 2 nhưng 2 không chia hết cho 3 nên đáp án đúng là "KHÔNG". 

Cuối cùng, các mảng có nhiều phần tử bằng nhau rất phức tạp. Nếu tất cả các số đều giống nhau thì câu trả lời luôn là "SI", vì bất kỳ phần tử nào cũng chia hết cho tất cả các phần tử khác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi phần tử trong mảng, hãy coi nó như một ước số ứng cử viên và kiểm tra xem nó có chia hết mọi phần tử khác hay không. Điều này đòi hỏi hai vòng lặp lồng nhau: một để chọn ứng cử viên và một để xác minh khả năng chia hết trên mảng. Mỗi lần kiểm tra có thời gian không đổi, do đó độ phức tạp của mỗi trường hợp kiểm thử là$O(n^2)$. Với$n = 10^4$, điều này dẫn đến$10^8$hoạt động trên mỗi trường hợp thử nghiệm, điều này trở nên quá chậm trên nhiều trường hợp thử nghiệm. 

Quan sát quan trọng là một ứng cử viên hợp lệ phải chia mọi phần tử, bao gồm cả phần tử tối đa. Điều này ngay lập tức ngụ ý rằng ứng cử viên phải nhỏ hơn hoặc bằng mọi phần tử mà nó chia, do đó nó phải là giá trị nhỏ nhất trong mảng. Tuy nhiên, chỉ riêng mức tối thiểu là không đủ trong lý luận tổng quát trừ khi chúng ta kiểm tra tính chia hết một cách rõ ràng. 

Điều này làm giảm đáng kể không gian tìm kiếm. Thay vì kiểm tra từng phần tử, chúng ta chỉ cần kiểm tra giá trị nhỏ nhất trong mảng. Nếu giá trị này chia cho mọi phần tử thì đó là câu trả lời hợp lệ. Mặt khác, không có phần tử nào khác có thể hoạt động được, bởi vì bất kỳ ứng cử viên nào lớn hơn sẽ không thể chia được mức tối thiểu. 

Điều này biến vấn đề thành một lần quét duy nhất để tìm mức tối thiểu, sau đó là lần quét thứ hai để xác minh khả năng chia hết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc mảng số nguyên. Điều này tách biệt các kịch bản độc lập để không có trạng thái nào chuyển tiếp giữa chúng. 
2. Quét mảng một lần để tìm phần tử nhỏ nhất. Ứng cử viên này được chọn vì bất kỳ ước số hợp lệ nào cũng phải đủ nhỏ để chia tất cả các phần tử. 
3. Duyệt mảng một lần nữa và kiểm tra xem mọi phần tử có chia hết cho ứng viên này hay không. 
4. Nếu bất kỳ phần tử nào không vượt qua được việc kiểm tra tính chia hết, hãy kết luận ngay rằng không có phần tử hợp lệ nào tồn tại cho trường hợp kiểm thử này. 
5. Nếu tất cả các phần tử đều vượt qua kiểm tra, xuất ra "SI", nếu không thì xuất ra "NO". 

### Tại sao nó hoạt động 

Nếu một số chia hết mọi phần tử trong mảng thì nó phải chia hết phần tử nhỏ nhất. Vì không có số nào nhỏ hơn số tối thiểu tồn tại trong mảng nên ứng cử viên duy nhất có thể tồn tại trong mọi ràng buộc chính là giá trị tối thiểu. Bất kỳ số nào lớn hơn đều không thể chia nhỏ nhất, vì vậy nó không thể hợp lệ. Điều này tạo ra một thuộc tính duy nhất nghiêm ngặt: nhiều nhất một ứng cử viên cần được kiểm tra và tính hợp lệ của nó sẽ xác định đầy đủ câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))

        m = min(arr)

        ok = True
        for x in arr:
            if x % m != 0:
                ok = False
                break

        out.append("SI" if ok else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc tất cả các trường hợp thử nghiệm và xử lý chúng một cách độc lập. Đối với mỗi trường hợp, nó tính giá trị tối thiểu bằng một lần chuyển. 

Vòng lặp thứ hai kiểm tra khả năng chia hết của mọi phần tử theo mức tối thiểu này. Việc nghỉ sớm rất quan trọng vì nó tránh được những công việc không cần thiết khi tìm thấy một mẫu phản biện, cải thiện hiệu suất trong các đầu vào trong trường hợp xấu nhất khi xảy ra lỗi sớm. 

Một điểm tinh tế là chúng ta không cần kiểm tra xem mức tối thiểu có tự chia hay không, vì`x % m`luôn bằng 0 khi`x == m`. Điều này tránh logic dư thừa và giữ tính thống nhất trong kiểm tra trên tất cả các phần tử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
10 18
```Chúng tôi tính toán tối thiểu là 10. 

| Bước | Phần tử hiện tại | Tối thiểu (m) | Chia được? | Trạng thái | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 10 | vâng | tiếp tục | 
| 2 | 18 | 10 | không | thất bại | 

Vì 18 không chia hết cho 10 nên kết quả là "KHÔNG". 

Điều này khẳng định rằng mặc dù mức tối thiểu là một ứng cử viên đương nhiên nhưng nó không phải lúc nào cũng thỏa mãn điều kiện. 

### Ví dụ 2 

đầu vào:```
4 2 4 6 8 4
```Tối thiểu là 2. 

| Bước | Phần tử hiện tại | Tối thiểu (m) | Chia được? | Trạng thái | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 2 | vâng | tiếp tục | 
| 2 | 2 | 2 | vâng | tiếp tục | 
| 3 | 4 | 2 | vâng | tiếp tục | 
| 4 | 6 | 2 | vâng | tiếp tục | 
| 5 | 8 | 2 | vâng | tiếp tục | 
| 6 | 4 | 2 | vâng | tiếp tục | 

Tất cả các giá trị đều chia hết cho 2 nên đầu ra là "SI". 

Điều này chứng tỏ trường hợp phần tử tối thiểu đóng vai trò là ước số chung một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lượt để tìm mức tối thiểu và một lượt để xác minh tính chia hết | 
| Không gian | O(1) thêm | Chỉ có một vài biến ngoài lưu trữ đầu vào | 

Tổng công việc có tỷ lệ tuyến tính với số lượng phần tử trên tất cả các trường hợp thử nghiệm. Với tối đa 10.000 phần tử cho mỗi trường hợp thử nghiệm và 100 trường hợp thử nghiệm, giải pháp thực hiện tối đa khoảng một triệu thao tác, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from types import SimpleNamespace

    # inline solution
    input = sys.stdin.readline
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        m = min(arr)
        ok = True
        for x in arr:
            if x % m != 0:
                ok = False
                break
        res.append("SI" if ok else "NO")
    return "\n".join(res)

# provided samples
assert run("""4
2
10 18
6
2 7 3 7 8 7
6
4 6 1 3 4 1
6
4 2 4 6 8 4
""") == """NO
NO
SI
SI"""

# custom cases
assert run("""1
3
5 10 20
""") == "SI"

assert run("""1
3
3 6 10
""") == "NO"

assert run("""1
4
7 7 7 7
""") == "SI"

assert run("""1
5
2 3 4 5 6
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 10 20 | SI | tối thiểu là ước số hợp lệ | 
| 3 6 10 | KHÔNG | lỗi chia hết một phần | 
| 7 7 7 7 | SI | trường hợp đều bằng nhau | 
| 2 3 4 5 6 | KHÔNG | không tồn tại ước số hợp lệ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các phần tử đều giống hệt nhau. Đối với một đầu vào như`[7, 7, 7, 7]`, tối thiểu là 7 và mọi phần tử đều chia hết cho 7. Thuật toán quét một lần, xác nhận tính chia hết cho tất cả các phần tử và xuất ra "SI". Điều này xác minh rằng các giá trị lặp lại không yêu cầu bất kỳ xử lý đặc biệt nào. 

Một trường hợp khác là khi giá trị tối thiểu chỉ xuất hiện một lần và vẫn thất bại dưới dạng ước số, chẳng hạn như`[2, 3, 4, 6]`. Thuật toán chọn 2 làm ứng cử viên, nhưng ngay lập tức thất bại ở 3 vì`3 % 2 != 0`. Việc chấm dứt sớm sẽ ngăn cản việc kiểm tra không cần thiết. 

Trường hợp thứ ba là khi một số lớn hơn có vẻ hợp lý nhưng không thể đúng vì nó không thể chia nhỏ nhất. Vì`[4, 2, 4, 6, 8, 4]`, bất kỳ ứng cử viên nào khác ngoài 2 hoặc 4 đều không thể. Thuật toán xử lý chính xác trên 2 và xác thực nó dựa trên toàn bộ mảng, tạo ra "SI".
