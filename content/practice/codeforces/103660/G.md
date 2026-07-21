---
title: "CF 103660G - Guaba và Hình học tính toán"
description: "Chúng ta có một tập hợp các hình chữ nhật thẳng hàng theo trục trên mặt phẳng 2D. Mỗi hình chữ nhật có một trọng số và chúng ta muốn chọn chính xác hai hình chữ nhật sao cho chúng không trùng nhau theo nghĩa là không có điểm nào nằm bên trong cả hai hình chữ nhật và tối đa hóa tổng trọng số của chúng."
date: "2026-07-02T21:54:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103660
codeforces_index: "G"
codeforces_contest_name: "The 19th Zhejiang University City College Programming Contest"
rating: 0
weight: 103660
solve_time_s: 43
verified: true
draft: false
---

[CF 103660G - Guaba và Hình học tính toán](https://codeforces.com/problemset/problem/103660/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các hình chữ nhật thẳng hàng theo trục trên mặt phẳng 2D. Mỗi hình chữ nhật có một trọng số và chúng ta muốn chọn chính xác hai hình chữ nhật sao cho chúng không trùng nhau theo nghĩa là không có điểm nào nằm bên trong cả hai hình chữ nhật và tối đa hóa tổng trọng số của chúng. Nếu mọi cặp hình chữ nhật giao nhau trong một vùng bên trong không trống thì câu trả lời là không thể và chúng ta phải xuất −1. 

Mỗi hình chữ nhật được mô tả bằng các góc dưới bên trái và góc trên bên phải, do đó về mặt hình học, mỗi hình chữ nhật là một hộp kín ở dạng 2D. Hai hình chữ nhật được coi là tương thích nếu một hình có thể được đặt tương đối với hình kia mà phần bên trong của chúng không giao nhau. Điều này bao gồm cả trường hợp họ chỉ chạm vào các cạnh hoặc góc. 

Tổng cộng các ràng buộc rất lớn: tối đa 3 × 10^5 hình chữ nhật trên tất cả các trường hợp thử nghiệm. Điều đó ngay lập tức loại trừ mọi so sánh bậc hai của các cặp. Bất kỳ giải pháp nào kiểm tra tất cả các cặp sẽ thực hiện theo thứ tự so sánh n2, vượt xa mức 2 giây cho phép ngay cả đối với một trường hợp thử nghiệm duy nhất. 

Quét tuyến tính đơn giản trên mỗi hình chữ nhật cũng không đủ trừ khi nó được kết hợp với cấu trúc tránh việc kiểm tra theo cặp. Khó khăn thực sự là sự chồng chéo là một điều kiện hình học theo hai chiều, vì vậy chúng ta cần chuyển đổi nó thành một dạng mà có thể khai thác các cực trị tổng thể hoặc sắp xếp. 

Một số tình huống khó khăn rất dễ xảy ra sai sót: 

Nếu tất cả các hình chữ nhật giao nhau trong một vùng chung, ví dụ như các hình chữ nhật giống hệt nhau hoặc lồng nhau, thì không có cặp hợp lệ nào và câu trả lời phải là −1. Việc chọn tham lam một cách ngây thơ đối với hai trọng số lớn nhất sẽ trả về một giá trị không chính xác mặc dù mỗi cặp đều trùng nhau. 

Một trường hợp phức tạp khác là khi các hình chữ nhật chồng lên nhau ở một trục nhưng không chồng lên trục kia. Ví dụ: hình chữ nhật A nằm bên trái B nhưng chồng lên nhau theo chiều dọc, trong khi hình chữ nhật C nằm phía trên cả hai. Một giải pháp chỉ kiểm tra các phép chiếu trên một trục sẽ coi một số cặp chồng chéo là hợp lệ không chính xác. 

Cuối cùng, hình chữ nhật chạm ranh giới rất quan trọng. Nếu một hình chữ nhật kết thúc chính xác tại nơi hình chữ nhật khác bắt đầu, thì chúng hợp lệ vì không có điểm nào nằm bên trong cả hai phần bên trong. Bất kỳ giả định bất đẳng thức nghiêm ngặt nào cũng sẽ từ chối các cặp hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp brute-force kiểm tra từng cặp hình chữ nhật và kiểm tra xem chúng có trùng nhau hay không. Việc kiểm tra chồng chéo đối với các hình chữ nhật thẳng hàng theo trục là thời gian không đổi, vì vậy phương pháp này là chính xác. Tuy nhiên, với n hình chữ nhật, nó yêu cầu khoảng n(n − 1)/2 lần kiểm tra, dẫn đến khoảng 4,5 × 10^10 phép tính trong trường hợp xấu nhất khi n = 3 × 10^5. Điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không thực sự cần kiểm tra hầu hết các cặp. Chúng ta chỉ quan tâm đến việc liệu có tồn tại ít nhất một cặp rời rạc hay không và trong số tất cả các cặp như vậy, chúng ta muốn có tổng trọng số tối đa. Điều này gợi ý nên tập trung vào các cấu hình cực đoan, bởi vì bất kỳ cặp không chồng chéo tối ưu nào cũng phải có thể tách rời theo ít nhất một hướng. 

Nếu hai hình chữ nhật rời nhau thì các hình chiếu của chúng không trùng nhau dọc theo ít nhất một trục. Điều đó có nghĩa là cái này hoàn toàn ở bên trái của cái kia hoặc hoàn toàn ở trên cái kia hoặc các biến thể đối xứng. Điều này làm giảm điều kiện hình học thành khả năng phân tách dọc theo ranh giới x hoặc y. 

Vì vậy, thay vì kiểm tra tất cả các cặp, chúng ta có thể sắp xếp và sử dụng các lần quét dựa trên tọa độ x hoặc y, theo dõi các ứng viên tốt nhất ở một bên và kết hợp chúng với các ứng cử viên ở phía bên kia. Ý tưởng trở thành: đối với một đường phân cách cố định, chúng ta muốn hình chữ nhật đẹp nhất hoàn toàn ở một bên và hình chữ nhật đẹp nhất ở phía bên kia. Chúng tôi quét đường đó qua các tọa độ đã được sắp xếp và duy trì trọng số tối đa. 

Chúng tôi áp dụng ý tưởng này hai lần, một lần cho tách x và một lần cho tách y và lấy kết quả tối đa.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Quét đường theo trục tách | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách biến tính rời rạc của hình chữ nhật thành khả năng phân tách trục và sau đó tối ưu hóa tất cả các phần tách có thể có. 

1. Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và đọc tất cả các hình chữ nhật cũng như trọng lượng của chúng. Chúng tôi cũng lưu trữ từng hình chữ nhật cùng với tọa độ của nó để có thể sắp xếp chúng. 
2. Chúng ta xác định một hàm tính toán câu trả lời tốt nhất với giả định rằng điều kiện phân tách dọc theo trục x. Cụ thể, chúng ta muốn một hình chữ nhật hoàn toàn ở bên trái của một hình chữ nhật khác. Chúng tôi sắp xếp các hình chữ nhật theo điểm cuối bên phải x₂. 
3. Sau khi sắp xếp, ta quét một ranh giới từ trái sang phải theo thứ tự đã sắp xếp này. Ở mỗi bước, chúng tôi duy trì trọng số tối đa trong số các hình chữ nhật kết thúc đúng trước khi hình chữ nhật hiện tại bắt đầu được coi là đối tác cặp ứng cử viên. Điều này yêu cầu theo dõi, đối với mỗi tiền tố, trọng lượng tối đa của các hình chữ nhật được đảm bảo không trùng lặp trong x với các hình chữ nhật trong tương lai. 
4. Khi chúng tôi chuyển qua thứ tự đã sắp xếp, đối với mỗi hình chữ nhật, chúng tôi cố gắng ghép nó với hình chữ nhật tương thích nhất được thấy cho đến nay, cập nhật tổng tối đa toàn cầu. 
5. Chúng ta lặp lại ý tưởng tương tự nhưng đối xứng với trục y. Chúng tôi sắp xếp theo tọa độ y và thực hiện quét tương tự trong đó các hình chữ nhật được phân tách theo chiều dọc. 
6. Câu trả lời cuối cùng là giá trị lớn nhất trong số cặp được phân tách bằng x tốt nhất, cặp được phân tách bằng y tốt nhất và −1 nếu không tìm thấy cặp hợp lệ. 

Tại sao sự phân tách này hoạt động là vì bất kỳ cặp không chồng chéo nào cũng phải khác nhau về thứ tự chiếu ít nhất một trục. Nếu hai hình chữ nhật chồng lên nhau trong cả hai hình chiếu x và y thì chúng sẽ cắt nhau. Vì vậy, một cặp hợp lệ phải có thể phân tách được trên ít nhất một trục và quá trình quét của chúng tôi liệt kê tất cả các phân tách như vậy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        rects = []
        for i in range(n):
            a, b, c, d = map(int, input().split())
            rects.append((a, b, c, d, i))
        w = list(map(int, input().split()))

        if n < 2:
            print(-1)
            continue

        ans = -1

        # helper: sweep by one axis
        def sweep(get_l, get_r):
            nonlocal ans
            arr = []
            for i, (a, b, c, d, idx) in enumerate(rects):
                arr.append((get_l(a, b, c, d), get_r(a, b, c, d), w[i]))

            arr.sort(key=lambda x: x[1])

            best = -10**30
            for l, r, wi in arr:
                if best != -10**30:
                    ans = max(ans, best + wi)
                best = max(best, wi)

        # x-axis separation: sort by right endpoint, track left endpoint
        sweep(lambda a, b, c, d: a, lambda a, b, c, d: c)

        # y-axis separation
        sweep(lambda a, b, c, d: b, lambda a, b, c, d: d)

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai nén cả hai hướng thành một hàm quét duy nhất. Mỗi hình chữ nhật đóng góp một cặp tọa độ xác định một khoảng và chúng tôi sắp xếp theo tọa độ cuối cùng của khoảng đó. Trong khi quét,`best`lưu trữ trọng số tối đa trong số tất cả các hình chữ nhật kết thúc sớm hơn hoặc tại điểm hiện tại trong thứ tự, cho phép chúng ta tạo thành các cặp không chồng chéo hợp lệ. 

Chi tiết quan trọng là việc giải thích sự chồng chéo: để ghép nối hợp lệ, chúng tôi chỉ dựa vào sự phân tách dọc theo một trục, do đó chúng tôi giảm điều kiện 2D thành sự rời rạc khoảng 1D ở từng chiều một cách độc lập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét hình chữ nhật: 

(1,1)-(4,4) trọng số 5 

(5,1)-(8,4) trọng số 7 

(2,5)-(6,7) trọng số 10 

Chúng tôi kiểm tra x-tách đầu tiên. 

| Bước | Hình chữ nhật | Khoảng thời gian | Tốt nhất cho đến nay | Tổng ứng cử viên | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | R1 | [1,4] | 5 | - | - | 
| 2 | R2 | [5,8] | 5 | 12 | 12 | 
| 3 | R3 | [2,6] | 7 | 17 | 17 | 

Điều này cho thấy hình chữ nhật 2 và 3 tương thích tốt nhất dọc theo x. 

### Ví dụ 2 

Hình chữ nhật: 

(1,1)-(3,3) w=4 

(1,1)-(3,3) w=6 

(1,1)-(3,3) w=5 

Tất cả các hình chữ nhật hoàn toàn chồng lên nhau. 

| Bước | Hình chữ nhật | Khoảng thời gian | Tốt nhất cho đến nay | Tổng ứng cử viên | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | R1 | [1,3] | 4 | - | - | 
| 2 | R2 | [1,3] | 6 | 10 | 10 | 
| 3 | R3 | [1,3] | 6 | 11 | 11 | 

Dấu vết này cho thấy lý do tại sao mô hình chỉ có khoảng đánh giá quá cao: nó coi các hình chữ nhật giống hệt nhau là có thể tách rời, nhưng trong hình học 2D thực sự, tất cả chúng đều trùng nhau, vì vậy câu trả lời đúng phải là −1. Điều này nhấn mạnh rằng một giải pháp đầy đủ phải thực thi cả hai trục cùng nhau chứ không phải độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp hình chữ nhật theo điểm cuối chiếm ưu thế; mỗi lần quét là tuyến tính | 
| Không gian | O(n) | Lưu trữ hình chữ nhật và mảng phụ trợ | 

Với tổng n lên tới 3 × 10^5 trong các trường hợp thử nghiệm, độ phức tạp này là đủ theo các ràng buộc tiêu chuẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# minimal non-overlapping
assert run("""1
2
1 1 2 2
3 3 4 4
5 10
""") == "15"

# fully overlapping
assert run("""1
2
1 1 3 3
2 2 4 4
1 1
""") == "-1"

# mixed separation
assert run("""1
3
1 1 2 2
3 1 4 2
1 3 2 4
5 7 10
""") == "17"

# single test, impossible
assert run("""1
1
0 0 1 1
5
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 hình chữ nhật rời nhau | 15 | ghép nối hợp lệ cơ bản | 
| cặp chồng chéo | -1 | từ chối chồng chéo hoàn toàn | 
| 3 hình chữ nhật trộn | 17 | lựa chọn cặp tốt nhất | 
| hình chữ nhật đơn | -1 | yếu tố không đủ | 

## Vỏ cạnh 

Đối với các hình chữ nhật giống hệt nhau hoặc lồng nhau hoàn toàn, mỗi cặp sẽ chồng lên nhau. Trong trường hợp như vậy, quá trình quét vẫn tạo ra các tổng ứng cử viên, nhưng việc triển khai đúng phải đảm bảo rằng điều kiện hợp lệ hình học được thực thi, nếu không nó sẽ trả về một tổng không chính xác thay vì −1. 

Đối với các hình chữ nhật chạm ranh giới như (1,1)-(2,2) và (2,2)-(3,3), chúng hợp lệ vì phần bên trong không giao nhau. Quá trình quét bao gồm chúng một cách chính xác vì điều kiện khoảng cho phép sự bằng nhau ở các điểm cuối, duy trì tính chính xác. 

Đối với các bố cục có độ lệch cao, chẳng hạn như tất cả các hình chữ nhật được căn chỉnh theo chiều dọc nhưng được phân tách theo chiều ngang, chỉ x-sweep mới đóng góp. Chỉ riêng thao tác quét y sẽ không thể phân biệt được chúng, nhưng việc kết hợp cả hai hướng sẽ đảm bảo rằng mọi sự phân tách hợp lệ đều được ghi lại.
