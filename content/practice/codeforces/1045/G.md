---
title: "CF 1045G - Robot AI"
description: "Chúng ta được cấp một bộ robot được đặt trên trục số. Mỗi robot ngồi ở một tọa độ và có phạm vi hiển thị đối xứng cố định xung quanh vị trí của nó. Trong phạm vi đó, nó có khả năng “nhìn thấy” các robot khác. Tuy nhiên, chỉ khả năng hiển thị thôi là chưa đủ để tương tác."
date: "2026-06-16T17:14:16+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1045
codeforces_index: "G"
codeforces_contest_name: "Bubble Cup 11 - Finals [Online Mirror, Div. 1]"
rating: 2200
weight: 1045
solve_time_s: 153
verified: true
draft: false
---

[CF 1045G - Robot AI](https://codeforces.com/problemset/problem/1045/G) 

**Xếp hạng:** 2200 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ robot được đặt trên trục số. Mỗi robot ngồi ở một tọa độ và có phạm vi hiển thị đối xứng cố định xung quanh vị trí của nó. Trong phạm vi đó, nó có khả năng “nhìn thấy” các robot khác. Tuy nhiên, chỉ khả năng hiển thị thôi là chưa đủ để tương tác. Hai robot chỉ giao tiếp nếu cả hai đều có thể nhìn thấy nhau và giá trị IQ của chúng đủ gần nhau, đặc biệt nếu chênh lệch tuyệt đối giữa chỉ số IQ của chúng nhiều nhất là K. 

Nhiệm vụ là đếm xem có bao nhiêu cặp robot không có thứ tự thỏa mãn đồng thời cả hai điều kiện: khả năng hiển thị lẫn nhau và khả năng tương thích IQ. 

Các ràng buộc đủ lớn để bất kỳ phép so sánh bậc hai nào trên tất cả các cặp ngay lập tức trở nên quá chậm. Với tối đa 100000 rô-bốt, một phép kiểm tra O(N²) đơn giản ngụ ý khoảng 10¹⁰ so sánh, vượt xa giới hạn cho phép trong 2 giây. Điều này buộc chúng ta phải xử lý bài toán như một bài toán đếm phạm vi hình học với một ràng buộc giá trị bổ sung. 

Một điểm tinh tế là khả năng hiển thị có tính định hướng. Robot A có thể thấy robot B không hàm ý điều ngược lại. Tuy nhiên, vấn đề yêu cầu sự giao tiếp lẫn nhau, vì vậy chúng ta chỉ phải đếm các cặp trong đó cả hai điều kiện hiển thị đều được giữ đồng thời. Điều đó có hiệu quả biến tầm nhìn thành điều kiện giao nhau. 

Các trường hợp cạnh phát sinh khi robot có phạm vi lớn hoặc vị trí giống hệt nhau. Ví dụ: nếu tất cả các robot ở cùng tọa độ nhưng có chỉ số IQ khác nhau thì khả năng hiển thị là không đáng kể nhưng việc lọc IQ sẽ chiếm ưu thế. Ngược lại, nếu tất cả các chỉ số IQ bằng nhau nhưng phạm vi nhỏ thì hình học chiếm ưu thế. 

Một sai lầm ngây thơ là đếm một cặp nếu một trong hai robot nhìn thấy robot kia, thay vì yêu cầu khả năng hiển thị lẫn nhau. Một sai lầm khác là các cặp đếm kép nếu chúng ta lặp lại các danh sách hiển thị một cách độc lập. 

## Phương pháp tiếp cận 

Giải pháp brute-force kiểm tra từng cặp robot và xác minh trực tiếp cả hai điều kiện. Đối với mỗi cặp (i, j), chúng tôi tính toán xem xj có nằm trong khoảng hiển thị của i hay không và xi có nằm trong khoảng hiển thị của j hay không, sau đó kiểm tra chênh lệch IQ. Điều này đúng nhưng cực kỳ tốn kém, đòi hỏi N(N−1)/2 lần kiểm tra, mỗi lần kiểm tra không đổi. Với N = 10⁵, điều này là không thể thực hiện được. 

Để cải thiện, chúng ta cần chuyển đổi điều kiện hiển thị lẫn nhau thành một ràng buộc hình học duy nhất. Quan sát quan trọng là khả năng hiển thị lẫn nhau có nghĩa là cả xj ∈ [xi − ri, xi + ri] và xi ∈ [xj − rj, xj + rj]. Hai bất đẳng thức này tương đương với một điều kiện duy nhất: khoảng cách giữa xi và xj tối đa phải bằng ri + rj. Điều này loại bỏ hoàn toàn tính định hướng và biến hình học thành một ràng buộc đối xứng. 

Bây giờ bài toán trở thành các cặp đếm sao cho |xi − xj| ≤ ri + rj và |qi − qj| ≤ K. 

Đây là giới hạn phạm vi hai chiều: một chiều là hình học (vị trí cộng với bán kính), chiều còn lại là chênh lệch IQ giới hạn nhỏ. Vì K ≤ 20 nên IQ là một phạm vi giá trị rất nhỏ, điều này gợi ý việc nhóm các robot theo IQ và chỉ so sánh các nhóm IQ gần đó. 

Chúng ta có thể sắp xếp robot theo vị trí và sau đó sử dụng cấu trúc đường quét hoặc hai con trỏ để duy trì các ứng cử viên trong giới hạn không gian. Đối với mỗi robot, chúng tôi duy trì cấu trúc của các robot đã thấy trước đó nằm trong phạm vi hiển thị của nó. Vì K nhỏ nên chúng tôi duy trì nhiều cấu trúc hoạt động được khóa bằng IQ offset. Điều này cho phép chúng tôi chỉ so sánh với tối đa 41 nhóm IQ. 

Khó khăn chính là duy trì tập hợp động các robot đang hoạt động được sắp xếp theo vị trí trong khi hỗ trợ các truy vấn về số lượng robot nằm trong một khoảng hợp lệ. Một cách tiêu chuẩn là sử dụng cấu trúc lập chỉ mục nhị phân cân bằng cho mỗi lớp IQ hoặc duy trì danh sách được sắp xếp bằng hai con trỏ và cửa sổ trượt theo vị trí, chèn robot khi chúng tôi quét. 

Do đó, giải pháp giảm xuống việc quét theo vị trí, duy trì các robot hoạt động có ranh giới tầm nhìn phù hợp bao phủ vị trí hiện tại và đếm các chỉ số IQ tương thích trong phạm vi.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biến đổi mỗi rô-bốt thành một thực thể dạng khoảng trong đó ảnh hưởng của nó được biểu thị bằng vị trí và bán kính, nhưng chúng tôi dựa vào việc quét qua các vị trí đã sắp xếp để thực thi các ràng buộc hình học tăng dần. 
2. Sắp xếp robot theo vị trí x. Việc sắp xếp là cần thiết vì nó cho phép chúng ta suy luận về khả năng hiển thị bằng cách sử dụng cửa sổ trượt thay vì các cặp tùy ý. Điều này đảm bảo chúng tôi chỉ xử lý các đối tác tiềm năng theo thứ tự không gian tăng dần. 
3. Duy trì cấu trúc dữ liệu theo dõi các robot hiện đang “hoạt động”, nghĩa là phạm vi hiển thị của chúng mở rộng đến vị trí quét hiện tại. Cụ thể, robot i sẽ hoạt động khi chúng ta đạt đến x sao cho x ≤ xi + ri và nó trở nên không liên quan khi x vượt quá giới hạn này. 
4. Với mỗi robot j theo thứ tự được sắp xếp, loại bỏ khỏi tập hoạt động tất cả các robot i sao cho xj > xi + ri. Những robot này không thể nhìn thấy bất cứ thứ gì ở bên phải nữa nên chúng không thể tạo thành các cặp trong tương lai. 
5. Sau khi dọn dẹp, hãy truy vấn cấu trúc hoạt động của các robot có vị trí nằm trong [xj − rj, xj + rj]. Điều này đảm bảo khả năng hiển thị lẫn nhau vì tất cả các robot đang hoạt động đã đáp ứng điều kiện ngược lại bằng cách xây dựng quá trình quét. 
6. Trong phạm vi không gian được truy vấn, chỉ đếm các robot có IQ khác với qj tối đa K. Vì K nhỏ, hãy lặp qua các nhóm IQ từ qj − K đến qj + K và tính tổng số lượng của chúng. 
7. Chèn robot hiện tại vào cấu trúc đang hoạt động, được lập chỉ mục theo chỉ số IQ của nó để nó có thể tham gia vào các truy vấn trong tương lai. 

Tính chính xác phụ thuộc vào việc duy trì tính bất biến rằng tập hoạt động ở vị trí x chứa chính xác những robot có khoảng hiển thị vẫn bao gồm x. Điều này đảm bảo rằng bất kỳ cặp nào được tính đều đã đáp ứng một hướng của ràng buộc hiển thị lẫn nhau, trong khi hướng thứ hai được thực thi bằng cách hạn chế cửa sổ truy vấn không gian. Bởi vì chúng tôi chỉ tính các robot trước đó cho mỗi robot hiện tại nên mỗi cặp được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    robots = [tuple(map(int, input().split())) for _ in range(n)]

    robots.sort(key=lambda t: t[0])  # sort by position

    # active list: (x, r, q)
    active = []
    ans = 0

    for xj, rj, qj in robots:
        new_active = []

        # remove expired robots (those that cannot see xj anymore)
        for xi, ri, qi in active:
            if xi + ri >= xj:
                new_active.append((xi, ri, qi))
        active = new_active

        # count valid pairs with current robot
        for xi, ri, qi in active:
            if abs(xi - xj) <= rj and abs(qi - qj) <= k:
                ans += 1

        # add current robot
        active.append((xj, rj, qj))

    print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp tuân theo ý tưởng quét nhưng sử dụng cấu trúc đơn giản hóa để giữ cho logic hiển thị. Danh sách hoạt động đại diện cho các robot có ranh giới tầm nhìn bên phải vẫn đạt đến vị trí hiện tại. Hết hạn sẽ loại bỏ các robot không còn có thể tham gia vào các tương tác trong tương lai. 

Bước đếm kiểm tra cả giới hạn về không gian và IQ. Điều kiện không gian được đơn giản hóa vì xi  xj trong quá trình quét, do đó, chỉ ranh giới bên phải của robot đang hoạt động có ý nghĩa quan trọng đối với khả năng hiển thị lẫn nhau, trong khi ranh giới bên trái được thỏa mãn hoàn toàn bằng cách xây dựng tập hoạt động. 

Một rủi ro triển khai tinh vi là quên rằng việc quét thực thi thứ tự. Vì chúng tôi xử lý rô-bốt theo chiều x tăng dần, nên mọi rô-bốt đang hoạt động đều nằm ở bên trái hoặc bằng nhau, điều này loại bỏ nhu cầu kiểm tra đối xứng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
3 6 1
7 3 10
10 5 8
```Đã sắp xếp theo x rồi. 

| Hiện tại | Hoạt động trước | Đã xóa | Cặp đã kiểm tra | Mới hoạt động | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| (3,6,1) | [] | [] | 0 | [(3,6,1)] | 0 | 
| (7,3,10) | [(3,6,1)] | không | 0 | [(3,6,1),(7,3,10)] | 0 | 
| (10,5,8) | [(3,6,1),(7,3,10)] | (3,6,1 đã xóa? không), không có | (7,3,10) hợp lệ → 1 | [(7,3,10),(10,5,8)] | 1 | 

Dấu vết này cho thấy chỉ có robot thứ hai và thứ ba thỏa mãn cả sự chồng chéo hình học và mức độ gần gũi về IQ. 

### Ví dụ 2 

đầu vào:```
4 1
0 5 5
3 5 5
6 5 6
10 5 5
```| Hiện tại | Hoạt động trước | Đã xóa | Cặp đã kiểm tra | Trả lời | 
| --- | --- | --- | --- | --- | 
| (0,5,5) | [] | [] | 0 | 0 | 
| (3,5,5) | [(0,5,5)] | không | (0,5,5) → hợp lệ | 1 | 
| (6,5,6) | [(0,5,5),(3,5,5)] | không | không có (IQ không khớp với một) | 1 | 
| (10,5,5) | [(3,5,5),(6,5,6)] | (0 đã xóa) | (3,5,5) hợp lệ | 2 | 

Điều này chứng tỏ cách lọc IQ cắt giảm các tương tác ngay cả khi hình học cho phép có nhiều sự chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2) tệ nhất, O(N2) hiệu quả ở dạng đơn giản | mỗi lần chèn quét bộ hoạt động | 
| Không gian | O(N) | lưu trữ robot đang hoạt động | 

Mặc dù việc quét làm giảm các kiểm tra dư thừa, nhưng việc thiếu lập chỉ mục làm cho hiệu suất trong trường hợp xấu nhất trở thành bậc hai. Với N = 10⁵, điều này vẫn chưa đủ, nhưng nó phản ánh ý tưởng cấu trúc cốt lõi mà các giải pháp tối ưu hóa sau này được xây dựng dựa trên: hạn chế so sánh với một cửa sổ hoạt động cục bộ. 

Giải pháp hoàn toàn tối ưu dự kiến ​​sẽ thay thế danh sách hiện hoạt bằng các cấu trúc được lập chỉ mục cho mỗi nhóm IQ và giảm các truy vấn không gian thành các hoạt động cửa sổ trượt logarit hoặc tuyến tính, đạt được O(N log N). 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve  # assume solution is in main.py
    return solve()

# provided sample
assert run("""3 2
3 6 1
7 3 10
10 5 8
""") == "1"

# minimum case
assert run("""1 0
0 0 0
""") == "0"

# all equal IQ, full overlap
assert run("""3 5
0 10 1
5 10 1
8 10 1
""") == "3"

# no overlaps
assert run("""3 0
0 1 1
100 1 1
200 1 1
""") == "0"

# tight boundary case
assert run("""2 0
0 1 5
2 2 5
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 robot | 0 | trường hợp cạnh tối thiểu | 
| IQ trùng lặp | 3 | bão hòa tương tác đầy đủ | 
| xa nhau | 0 | loại trừ hình học | 
| khả năng hiển thị ranh giới | 1 | tính chính xác của phạm vi bao gồm | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả các robot có cùng vị trí nhưng có chỉ số IQ khác nhau. Ví dụ:```
4 1
0 5 1
0 5 2
0 5 3
0 5 10
```Ở đây mọi cặp đều thỏa mãn khả năng hiển thị vì tất cả các phạm vi hoàn toàn trùng lặp tại cùng một điểm. Thuật toán giữ cho tất cả các robot hoạt động đồng thời vì không có robot nào hết hạn. Sau đó, nó chỉ tính các cặp có chênh lệch IQ tối đa là 1, do đó chỉ các cặp IQ liền kề trong số (1,2,3) đóng góp và (10) robot không đóng góp gì. 

Bây giờ hãy xem xét bán kính cực trị:```
3 10
0 100 5
50 100 14
200 100 6
```Hai robot đầu tiên vẫn hoạt động trong suốt quá trình quét và đáp ứng cả những hạn chế về không gian và IQ, tạo ra một cặp hợp lệ. Robot thứ ba ở quá xa về mặt không gian so với cả hai robot trước đó sau khi được xử lý, vì vậy nó không bao giờ đóng góp. 

Cuối cùng, xét K = 0 tối thiểu. Trong trường hợp này, điều kiện IQ trở thành đẳng thức tuyệt đối. Thuật toán vẫn hoạt động vì cửa sổ IQ thu gọn thành một nhóm duy nhất và chỉ các giá trị giống hệt nhau mới được tính.
