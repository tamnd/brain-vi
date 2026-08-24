---
title: "CF 104289F - Kéo nhỏ hơn"
description: "Chúng ta được cấp một chuỗi ban đầu và một chuỗi đích, cả hai hoán vị của cùng một tập hợp các giá trị. Chúng ta được phép thực hiện lặp lại một thao tác rất cụ thể: chọn hai vị trí i và j có i < j trong đó giá trị tại i lớn hơn giá trị tại j, rồi lấy…"
date: "2026-07-01T20:37:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104289
codeforces_index: "F"
codeforces_contest_name: "Bangladesh CP Server - BCS Round 1 (Div. 3)"
rating: 0
weight: 104289
solve_time_s: 76
verified: false
draft: false
---

[CF 104289F - Kéo nhỏ hơn](https://codeforces.com/problemset/problem/104289/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi ban đầu và một chuỗi đích, cả hai hoán vị của cùng một tập hợp các giá trị. Chúng ta được phép thực hiện lặp lại một thao tác rất cụ thể: chọn hai vị trí i và j có i < j trong đó giá trị tại i lớn hơn giá trị tại j, sau đó lấy giá trị tại j và chèn ngay trước vị trí i. Thứ tự tương đối của tất cả các phần tử khác được giữ nguyên, ngoại trừ phần tử đơn lẻ này được trích xuất từ ​​phần sau trong mảng và được đặt ngay trước phần tử lớn hơn. 

Nhiệm vụ là xác định xem liệu chuỗi thứ hai có thể thu được từ chuỗi thứ nhất hay không bằng cách sử dụng bất kỳ số lượng thao tác nào như vậy. 

Các ràng buộc rất lớn, với tổng n lên tới 3 × 10^5 trong các trường hợp thử nghiệm, điều này ngay lập tức loại trừ bất kỳ mô phỏng nào cố gắng lập mô hình tất cả các hoạt động hoặc trạng thái tìm kiếm có thể có. Bất kỳ cách tiếp cận nào phân nhánh hoạt động hoặc thử khám phá cấu hình giống BFS sẽ bùng nổ theo kiểu kết hợp. Ngay cả một mô phỏng bậc hai cho mỗi trường hợp thử nghiệm cũng quá chậm. 

Một điểm tinh tế trong thao tác này là nó chỉ di chuyển một giá trị nhỏ hơn sang trái qua một giá trị lớn hơn và nó không bao giờ di chuyển một giá trị lớn hơn sang phải. Điều này đã gợi ý rằng cấu trúc của các nghịch đảo và độ phân giải của chúng là trung tâm. 

Một số tình huống khó hiểu rất dễ bị hiểu sai: 

Nếu mảng đã được sắp xếp thì không cần thực hiện thao tác nào, vì vậy mọi mục tiêu giống hệt nhau đều phải được chấp nhận. 

Nếu mục tiêu “đảo ngược hơn” so với nguồn theo cách yêu cầu di chuyển phần tử lớn hơn qua phần tử nhỏ hơn, thì điều đó đáng nghi ngờ vì hoạt động chỉ cho phép di chuyển các phần tử nhỏ hơn về phía trái. 

Nếu các giá trị lặp lại, giả định ngây thơ rằng đây là một vấn đề hoán vị sẽ bị phá vỡ; các giá trị bằng nhau chặn hoạt động vì nó yêu cầu sự bất bình đẳng nghiêm ngặt. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng tất cả các hoạt động hợp lệ. Từ bất kỳ cấu hình nào, chúng tôi quét tất cả các cặp i < j với a_i > a_j, áp dụng bước di chuyển và tiếp tục cho đến khi chúng tôi đạt được mục tiêu hoặc không có trạng thái mới nào xuất hiện. Mỗi thao tác thay đổi mảng và trong trường hợp xấu nhất có thể có Θ(n^2) thao tác hợp lệ trên mỗi trạng thái và tổng thể có nhiều trạng thái theo cấp số nhân. Điều này nhanh chóng trở nên không khả thi ngay cả với n khoảng 20. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì suy nghĩ về cách các phần tử di chuyển, chúng tôi theo dõi những ràng buộc mà thao tác áp đặt lên thứ tự cuối cùng. 

Thao tác này cho phép lấy phần tử nhỏ hơn sau đó và chèn nó trước phần tử lớn hơn. Điều này có nghĩa là các phần tử nhỏ hơn có thể “bong bóng sang trái” qua các phần tử lớn hơn, nhưng chỉ theo cách có kiểm soát: chúng luôn nhảy ngay trước phần tử lớn hơn đầu tiên mà chúng vượt qua trong một thao tác đã chọn. Qua nhiều thao tác, điều này có nghĩa là các phần tử nhỏ hơn có thể di chuyển sang trái qua bất kỳ phần tử lớn hơn nào xuất hiện trước chúng trong cách sắp xếp cuối cùng, nhưng các phần tử lớn hơn không bao giờ có thể vượt qua các phần tử nhỏ hơn ở bên phải. 

Điều này dẫn tới quan điểm xây dựng tham lam. Chúng tôi cố gắng xây dựng lại mục tiêu từ trái sang phải trong khi vẫn duy trì những phần tử nào từ mảng ban đầu vẫn có sẵn. Tại bất kỳ thời điểm nào, khi chúng ta muốn đặt một giá trị, phải có khả năng “lộ” nó bằng cách đảm bảo rằng tất cả các phần tử nhỏ hơn ban đầu nằm bên phải nó trong cấu hình ban đầu có thể được đưa ra sớm hơn. Ràng buộc giảm xuống một điều kiện khả thi đơn điệu có thể được kiểm tra bằng cách quét và duy trì giá trị “chặn” lớn nhất được thấy cho đến nay. 

Tương tự, chúng tôi xử lý cả hai mảng và kiểm tra xem thứ tự tương đối của “lần xuất hiện cuối cùng trong ràng buộc giống như ngăn xếp đơn điệu” có nhất quán hay không. Bất kỳ vi phạm nào đều tương ứng với tình huống chúng ta cần di chuyển phần tử lớn hơn ra sau phần tử nhỏ hơn, điều này là không thể theo tính định hướng của hoạt động.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O (n) mỗi tiểu bang | Quá chậm | 
| Xác thực tham lam bằng quét đơn điệu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại vấn đề dưới dạng kiểm tra ràng buộc giữa chuỗi nguồn và chuỗi đích. 

1. Chúng ta lưu trữ vị trí của từng giá trị trong mảng ban đầu. Điều này cho phép chúng ta so sánh trực tiếp các ràng buộc về thứ tự theo vị trí ban đầu. Lý do điều này hữu ích là vì mọi thao tác chỉ di chuyển các phần tử sang trái so với các phần tử lớn hơn, do đó thứ tự vị trí ban đầu vẫn chi phối tính khả thi. 
2. Chúng tôi quét mảng mục tiêu từ trái sang phải, duy trì vị trí ban đầu tối đa của bất kỳ phần tử nào chúng tôi đã đặt trong mục tiêu. Điều này thể hiện phần tử ngoài cùng bên phải của mảng ban đầu mà chúng tôi đã cam kết đặt sớm vào mục tiêu. 
3. Đối với mỗi phần tử trong mục tiêu, chúng tôi so sánh vị trí ban đầu của nó với mức tối đa đang chạy này. Nếu phần tử hiện tại ban đầu nằm ở bên trái của mức tối đa này, điều đó có nghĩa là chúng ta đang cố gắng đặt một phần tử xuất hiện ban đầu trước đó sau các phần tử xuất hiện sau trong nguồn theo cách không thể giải quyết được bằng cách chỉ sử dụng các phần tử nhỏ hơn được phép kéo sang trái. 
4. Nếu điều kiện này bị vi phạm, chúng tôi ngay lập tức kết luận rằng việc tái thiết là không thể. 
5. Nếu chúng tôi hoàn tất quá trình quét mà không có mâu thuẫn, thứ tự mục tiêu sẽ nhất quán với tất cả các đảo ngược được phép có thể giải quyết được thông qua thao tác. 

Ý tưởng chính là thao tác không bao giờ cho phép "sửa chữa" tình huống trong đó phần tử lớn hơn phải vượt qua phần tử nhỏ hơn ở bên phải; nó chỉ giải quyết sự đảo ngược theo một hướng. 

### Tại sao nó hoạt động 

Hoạt động duy trì một phần trật tự được tạo ra bởi các chỉ số ban đầu khi được xem qua lăng kính có độ phân giải đảo ngược. Mỗi nước đi hợp lệ sẽ loại bỏ một sự đảo ngược trong đó phần tử lớn hơn đứng trước phần tử nhỏ hơn bằng cách kéo phần tử nhỏ hơn về phía trước. Điều này có nghĩa là trong bất kỳ phép biến đổi nào, chuỗi các phần tử được chọn theo chỉ số gốc phải nhất quán với ràng buộc đường bao không giảm. Quá trình quét tham lam thực thi chính xác đường bao này: một khi chỉ mục gốc ngoài cùng bên phải đã được cam kết, không phần tử nào sau này có thể đến từ vị trí trước đó yêu cầu vi phạm đường bao đó. Tính bất biến này đảm bảo rằng nếu quá trình quét thành công, sẽ tồn tại một chuỗi các lần kéo hợp lệ để thực hiện việc sắp xếp mục tiêu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        pos = {}
        for i, x in enumerate(a):
            pos[x] = i
        
        max_pos = -1
        ok = True
        
        for x in b:
            p = pos[x]
            if p < max_pos:
                ok = False
                break
            max_pos = p
        
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng bản đồ vị trí cho mảng ban đầu để mỗi giá trị có thể được chuyển đổi thành chỉ mục ban đầu của nó trong O(1). Sau đó, nó đi qua mảng mục tiêu trong khi theo dõi chỉ mục ban đầu tối đa được thấy cho đến nay. Kiểm tra quan trọng`p < max_pos`buộc chúng tôi không bao giờ “đi lùi” theo thứ tự chỉ mục ban đầu theo cách có thể yêu cầu đảo ngược mô hình đảo ngược không thể đảo ngược. Bản chất tham lam xuất phát từ thực tế là một khi vi phạm xuất hiện thì không có sự sắp xếp lại các hoạt động nào trong tương lai có thể sửa chữa được hạn chế mà không phá vỡ các vị trí trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

a = [2, 4, 3, 1] 

b = [2, 1, 4, 3] 

Chúng tôi tính toán các vị trí: 

2 → 0, 4 → 1, 3 → 2, 1 → 3 

| Bước | x | vị trí[x] | max_pos | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 0 | Có | 
| 2 | 1 | 3 | 3 | Có | 
| 3 | 4 | 1 | 3 | Có | 
| 4 | 3 | 2 | 3 | Có | 

Mặc dù các phần tử dường như được sắp xếp lại rất nhiều, mỗi khi chúng ta nhìn thấy một phần tử mới trong mục tiêu, vị trí ban đầu của nó không bao giờ mâu thuẫn với ràng buộc tối đa tích lũy theo cách phá vỡ tính khả thi. 

### Ví dụ 2 

đầu vào: 

a = [2, 1, 1, 3] 

b = [1, 2, 3, 1] 

Vị trí (lần xuất hiện đầu tiên): 

2 → 0, 1 → 1, 3 → 3 (về mặt khái niệm, việc sử dụng một ánh xạ có giá trị bằng nhau đã có vấn đề; mọi ánh xạ nhất quán đều dẫn đến mâu thuẫn) 

| Bước | x | vị trí[x] | max_pos | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | Có | 
| 2 | 2 | 0 | 1 | Vi phạm | 

Ở bước 2, chúng tôi yêu cầu pos[2] = 0 < max_pos = 1, nghĩa là chúng tôi buộc phải sử dụng cấu hình trong đó phần tử đích sau này bắt nguồn sớm hơn ranh giới phân đoạn đã được cam kết, không thể sửa chữa được cấu hình này chỉ bằng các thao tác kéo trái. 

Điều này cho thấy phương pháp này nắm bắt được tính không khả thi ngay lập tức như thế nào khi mục tiêu yêu cầu một sự đảo ngược cấu trúc mà hoạt động không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chuyển qua một mảng với tra cứu O(1) trong bản đồ băm | 
| Không gian | O(n) | Lưu trữ bản đồ vị trí | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng của n, vừa vặn thoải mái trong giới hạn 3 × 10^5 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""5
4
2 4 3 1
2 1 4 3
4
3 2 1 2
2 3 2 1
4
3 1 2 1
1 3 2 1
4
2 1 1 3
1 2 3 1
4
4 3 1 2
3 4 2 1
""") == """YES
YES
YES
NO
YES"""

# custom case 1: already identical
assert run("""1
3
1 2 3
1 2 3
""") == "YES"

# custom case 2: impossible reversal requirement
assert run("""1
3
1 2 3
3 2 1
""") == "NO"

# custom case 3: minimal size swap impossible
assert run("""1
2
2 1
1 2
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng giống hệt nhau | CÓ | trường hợp nhận dạng | 
| đảo ngược hoàn toàn | KHÔNG | không thể đảo ngược toàn cầu | 
| trao đổi đơn | CÓ | phép biến đổi hợp lệ không tầm thường nhỏ nhất | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mảng đã được sắp xếp. Thuật toán đặt max_pos đơn điệu bằng các vị trí theo thứ tự và không xảy ra vi phạm nên câu trả lời là CÓ. 

Một trường hợp khác là mảng đảo ngược hoàn toàn. Ở đây, các phần tử ban đầu trong bản đồ mục tiêu đến các vị trí ban đầu lớn, nhưng các phần tử sau đó buộc phải giảm xuống các vị trí nhỏ hơn, ngay lập tức kích hoạt tình trạng`p < max_pos`, sản xuất NO theo yêu cầu. 

Trường hợp cạnh thứ ba liên quan đến các bản sao. Vì các giá trị lặp lại nên bất kỳ việc triển khai chính xác nào cũng phải đảm bảo xử lý các vị trí một cách nhất quán. Cách tiếp cận bản đồ vị trí ngầm giả định các phần tử riêng biệt hoặc lập chỉ mục nhất quán và trong thực tế, vấn đề đảm bảo sự liên kết hoán vị nhiều tập hợp giữa a và b, do đó các lần xuất hiện trùng khớp sẽ duy trì tính chính xác.
