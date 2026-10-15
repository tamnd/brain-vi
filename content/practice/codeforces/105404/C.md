---
title: "CF 105404C - Trò chơi có Queta"
description: "Chúng ta có một đoạn thẳng được đánh dấu bằng hai điểm cố định. Bên trong đoạn này có một số đối tượng, mỗi đối tượng được đặt ở một tọa độ riêng biệt. Mọi vật thể ban đầu đều quay mặt sang trái về phía dấu thứ nhất hoặc sang phải về phía dấu thứ hai."
date: "2026-06-23T04:48:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105404
codeforces_index: "C"
codeforces_contest_name: "XXIV Spain Olympiad in Informatics, Online Qualifier 2"
rating: 0
weight: 105404
solve_time_s: 74
verified: true
draft: false
---

[CF 105404C - Trò chơi có Queta](https://codeforces.com/problemset/problem/105404/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đoạn thẳng được đánh dấu bằng hai điểm cố định. Bên trong đoạn này có một số đối tượng, mỗi đối tượng được đặt ở một tọa độ riêng biệt. Mọi vật thể ban đầu đều quay mặt sang trái về phía dấu thứ nhất hoặc sang phải về phía dấu thứ hai. Khi quá trình bắt đầu, tất cả các đối tượng sẽ di chuyển đồng thời với tốc độ như nhau theo hướng đã chọn. Khi hai vật chạm nhau, cả hai đều biến mất ngay lập tức. Nếu một đối tượng đạt đến một trong hai ranh giới mà không gặp bất cứ điều gì, nó chỉ cần rời khỏi đoạn đó một cách an toàn. 

Quá trình tiếp tục cho đến khi không còn đối tượng nào bên trong phân đoạn. Nhiệm vụ là xác định có bao nhiêu vật thể sống sót sau toàn bộ tương tác này, nghĩa là cuối cùng chúng sẽ đến được một trong hai điểm cuối mà không bị phá hủy trong một vụ va chạm. 

Các ràng buộc cho phép tối đa 10.000 đối tượng cho mỗi trường hợp thử nghiệm và nhiều trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi mô phỏng theo dõi chuyển động liên tục hoặc giải quyết va chạm theo thứ tự sự kiện. Một mô phỏng theo cặp đơn giản sẽ là bậc hai trong mỗi trường hợp, dẫn đến khoảng 10^8 thao tác trong trường hợp xấu nhất, là đường biên hoặc quá chậm trong Python trong giới hạn thời gian chặt chẽ. 

Một trường hợp tế nhị quan trọng phát sinh khi các vật chuyển động cùng hướng ở cạnh nhau. Ví dụ: nếu cả hai đối tượng đều di chuyển sang phải, chúng không bao giờ tương tác, nhưng một mô phỏng đơn giản chỉ xem xét các va chạm gần nhất có thể cố gắng ghép chúng một cách không chính xác. Một trường hợp tinh vi khác là các hướng luân phiên, trong đó mọi vật thể có thể bị hủy bởi một động cơ đối diện gần đó, nhưng chỉ khi có logic ghép nối chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng chuyển động hoặc liên tục tìm ra sự kiện va chạm tiếp theo. Ở mỗi bước, chúng ta sẽ xác định tất cả các cặp đối tượng liền kề đang chuyển động hướng về nhau, tính toán va chạm sớm nhất giữa chúng, loại bỏ cả hai và tiếp tục. Điều này đúng về mặt khái niệm vì va chạm chỉ xảy ra giữa các động cơ đối lập lân cận. Tuy nhiên, việc duy trì tính kề cận và liên tục quét tìm xung đột sẽ dẫn đến hành vi O(n^2) trong trường hợp xấu nhất, vì mỗi lần xóa sẽ kích hoạt cập nhật và quét lại. 

Nhận xét quan trọng là các vị trí tuyệt đối không liên quan; chỉ có vấn đề thứ tự tương đối. Khi chúng ta sắp xếp các đối tượng theo vị trí, hệ thống sẽ trở thành một chuỗi một chiều trong đó mỗi đối tượng sẽ di chuyển sang trái hoặc phải. Va chạm chỉ có thể xảy ra giữa một vật chuyển động sang phải và một vật chuyển động sang trái nằm ở bên phải của nó, và đặc biệt hơn, các va chạm luôn xảy ra theo kiểu ghép đôi giống như chồng. 

Nếu chúng ta quét từ trái sang phải, mọi vật chuyển động sang phải có thể được coi là “chờ” một va chạm tiềm tàng với một vật chuyển động trái trong tương lai. Một vật thể chuyển động sang trái gặp vật thể chuyển động phải không thể so sánh được gần nhất hoặc thoát ra nếu không tồn tại. Đây chính xác là cấu trúc của quá trình hủy ngăn xếp. 

Do đó, thay vì mô phỏng thời gian, chúng tôi xử lý các đối tượng theo thứ tự được sắp xếp và duy trì một chồng các đối tượng chuyển động sang phải. Khi chúng ta nhìn thấy một vật chuyển động sang trái, nó sẽ hủy bỏ bằng một vật chuyển động sang phải nếu có; nếu không, nó sẽ tồn tại. Các đối tượng chuyển động sang phải chỉ đơn giản được đẩy vào ngăn xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Quét ngăn xếp | O(n log n) | O(n) | Đã chấp nhận | 

Yếu tố logarit chỉ đến từ việc sắp xếp các vị trí. 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi ghép từng đối tượng với vị trí và hướng của nó, sau đó sắp xếp chúng theo vị trí để xử lý chúng theo thứ tự không gian.

1. Sắp xếp tất cả các đối tượng theo tọa độ của chúng. Điều này đảm bảo chúng tôi tôn trọng thứ tự tương tác vật lý từ trái sang phải, vì các va chạm chỉ phụ thuộc vào vị trí tương đối. 
2. Duy trì một ngăn xếp trống để lưu trữ các đối tượng hiện đang di chuyển sang bên phải và chờ các xung đột có thể xảy ra. 
3. Lặp lại các đối tượng từ trái sang phải. Đối với mỗi đối tượng, chúng tôi kiểm tra hướng của nó. 
4. Nếu đối tượng di chuyển sang phải, chúng ta đẩy nó vào ngăn xếp vì sau này nó có thể va chạm với một đối tượng chuyển động sang trái. 
5. Nếu đối tượng di chuyển sang trái, chúng tôi cố gắng giải quyết xung đột: trong khi có một đối tượng chuyển động sang phải trong ngăn xếp, chúng tôi loại bỏ nó và cũng loại bỏ đối tượng chuyển động sang trái hiện tại, vì chúng tiêu diệt lẫn nhau. Sự va chạm xảy ra ngay lập tức về mặt logic, do đó không tham gia thêm nữa. 
6. Nếu ngăn xếp trở nên trống trước khi đối tượng chuyển động trái hiện tại bị xóa, thì đối tượng chuyển động trái này vẫn tồn tại và di chuyển ra khỏi đoạn tới ranh giới bên trái một cách hiệu quả. 
7. Sau khi xử lý tất cả các đối tượng, mọi đối tượng di chuyển sang phải còn lại trong ngăn xếp vẫn tồn tại và mọi đối tượng di chuyển sang trái chưa bao giờ bị hủy cũng được tính. 

Câu trả lời cuối cùng là số lượng đồ vật còn sót lại. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ đối tượng chuyển động sang phải. Nó chỉ có thể va chạm với các vật thể ở bên phải di chuyển sang trái. Trong số đó, vật đầu tiên nó gặp đúng lúc chính xác là vật thể chuyển động sang trái gần nhất về phía phải theo thứ tự không gian. Ngăn xếp đảm bảo chúng tôi luôn khớp đối tượng chuyển động phải chưa được giải quyết gần nhất với đối tượng chuyển động trái tiếp theo. Bởi vì cả hai đều di chuyển với tốc độ như nhau nên không thể thực hiện được sự sắp xếp lại trung gian: trật tự được giữ nguyên và việc ghép đôi diễn ra rất tham lam nhưng an toàn. Mỗi lần hủy tương ứng với một sự kiện va chạm duy nhất và không có va chạm hợp lệ nào bị bỏ qua hoặc tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m1, m2 = map(int, input().split())
    pos = list(map(int, input().split()))
    dir_ = list(map(int, input().split()))

    arr = list(zip(pos, dir_))
    arr.sort()

    stack_right = 0
    survivors_left = 0

    for _, d in arr:
        if d == 2:
            stack_right += 1
        else:
            if stack_right > 0:
                stack_right -= 1
            else:
                survivors_left += 1

    print(stack_right + survivors_left)
```Bước sắp xếp là cần thiết vì thứ tự đầu vào thô không phản ánh các tương tác không gian. Sau khi sắp xếp, chúng tôi thu gọn động lực thành một lượt duy nhất. Biến`stack_right`đại diện cho các đối tượng chuyển động sang phải chưa được giải quyết và chưa gặp đối tượng chuyển động sang trái. Biến`survivors_left`theo dõi các vật thể chuyển động sang trái mà chưa bao giờ gặp phải vật thể chuyển động phải trước khi đến ranh giới. 

Một lỗi phổ biến là giả sử tính đối xứng và cố gắng mô phỏng cả hai hướng một cách độc lập, nhưng sự tương tác chính xác phụ thuộc hoàn toàn vào thứ tự xử lý từ trái sang phải. Một điểm tinh tế khác là chúng ta không bao giờ cần mô phỏng các vị thế thực tế theo thời gian; phương hướng và thứ tự quyết định đầy đủ kết quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 2 16
14 4 11 8 6 9
1 1 2 1 1 2
```Các cặp được sắp xếp: 

| Vị trí | Hướng | Ngăn xếp (R) | Những người sống sót còn lại | Hành động | 
| --- | --- | --- | --- | --- | 
| 4 | L | 0 | 1 | tồn tại | 
| 6 | L | 0 | 2 | tồn tại | 
| 8 | L | 0 | 3 | tồn tại | 
| 9 | R | 1 | 3 | đẩy | 
| 11 | R | 2 | 3 | đẩy | 
| 14 | L | 1 | 3 | hủy một R | 

Cuối cùng: stack_right = 1, Survivor_left = 3, đáp án = 4 

Điều này cho thấy các động cơ bên trái tích lũy tự do như thế nào cho đến khi chúng gặp các động cơ bên phải và sự hủy bỏ chỉ xảy ra khi cả hai loại đều tồn tại. 

### Ví dụ 2 

đầu vào:```
5 1 19
11 16 8 7 4
2 1 1 2 2
```Các cặp được sắp xếp: 

| Vị trí | Hướng | Ngăn xếp (R) | Những người sống sót còn lại | Hành động | 
| --- | --- | --- | --- | --- | 
| 4 | R | 1 | 0 | đẩy | 
| 7 | R | 2 | 0 | đẩy | 
| 8 | L | 1 | 0 | hủy bỏ | 
| 11 | L | 0 | 0 | hủy bỏ | 
| 16 | R | 1 | 0 | đẩy | 

Câu trả lời cuối cùng là 1. 

Trường hợp này thể hiện sự hủy bỏ xen kẽ trong đó ngăn xếp liên tục hấp thụ các động lực bên trái cho đến khi cạn kiệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian | O(n) | lưu trữ các bộ đếm mảng và ngăn xếp được ghép nối | 

Với n tối đa 10.000 cho mỗi trường hợp thử nghiệm và tối đa 100 trường hợp thử nghiệm, điều này diễn ra thoải mái trong giới hạn. Chi phí sắp xếp vẫn có thể quản lý được và quá trình quét là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m1, m2 = map(int, input().split())
        pos = list(map(int, input().split()))
        dir_ = list(map(int, input().split()))

        arr = list(zip(pos, dir_))
        arr.sort()

        stack_right = 0
        survivors_left = 0

        for _, d in arr:
            if d == 2:
                stack_right += 1
            else:
                if stack_right:
                    stack_right -= 1
                else:
                    survivors_left += 1

        out.append(str(stack_right + survivors_left))

    return "\n".join(out)

input_data = """4
6 2 16
14 4 11 8 6 9
1 1 2 1 1 2
6 1 20
14 4 5 9 7 10
2 2 1 2 2 2
5 1 19
11 16 8 7 4
2 1 1 2 2
4 1 20
3 15 10 12
1 1 1 1
"""

expected_output = """4
4
1
4"""

assert run(input_data) == expected_output

# minimum size
assert run("""1
1 1 10
5
1
""") == "1"

# all same direction right
assert run("""1
3 0 10
2 5 7
2 2 2
""") == "3"

# alternating full cancellation
assert run("""1
4 0 10
1 2 3 4
2 1 2 1
""") == "0"

# left-heavy case
assert run("""1
5 0 10
1 2 3 4 5
1 1 1 1 1
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp sinh tồn căn cứ | 
| được rồi | n | không va chạm | 
| xen kẽ | 0 | chuỗi hủy bỏ đầy đủ | 
| tất cả còn lại | n | hành vi thoát ranh giới | 

## Vỏ cạnh 

Trường hợp một đối tượng tối thiểu xác nhận rằng thuật toán không vô tình yêu cầu ghép nối để tồn tại. Với một đối tượng, ngăn xếp vẫn trống và nó được tính trực tiếp là đối tượng sống sót. 

Trường hợp tất cả các vật thể chuyển động theo cùng một hướng xác minh rằng không có va chạm nhân tạo nào xảy ra. Vì chúng tôi không bao giờ so sánh các đối tượng cùng hướng nên ngăn xếp chỉ tăng lên hoặc không được sử dụng, tạo ra kết quả tồn tại hoàn toàn chính xác. 

Một cấu hình xen kẽ hoàn toàn nhấn mạnh logic hủy bỏ. Mỗi đối tượng chuyển động sang phải ngay lập tức được ghép nối với đối tượng chuyển động sang trái tiếp theo theo trình tự. Ngăn xếp thực thi chính xác sự hủy diệt một-một và số đếm cuối cùng trở thành 0 khi các cặp được cân bằng hoàn hảo. 

Một trường hợp có tất cả các đối tượng di chuyển sang trái đảm bảo rằng ngăn xếp vẫn trống xuyên suốt. Mọi đối tượng đều được phân loại là vật thể sống sót, xác nhận rằng thuật toán không nhầm lẫn khi yêu cầu một đối tượng chuyển động sang phải phù hợp để sinh tồn.
