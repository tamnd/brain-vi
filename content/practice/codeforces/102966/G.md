---
title: "CF 102966G - Goombas Va Chạm"
description: "Chúng ta được cung cấp một nền thẳng có thể được coi là một đoạn thẳng một chiều từ 0 đến L. Một số Goombas bắt đầu ở các vị trí số nguyên riêng biệt bên trong đoạn này."
date: "2026-07-04T06:40:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102966
codeforces_index: "G"
codeforces_contest_name: "2020-2021 ICPC - Gran Premio de Mexico - Repechaje"
rating: 0
weight: 102966
solve_time_s: 37
verified: true
draft: false
---

[CF 102966G - Goombas Va chạm](https://codeforces.com/problemset/problem/102966/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một nền thẳng có thể được coi là một đoạn thẳng một chiều từ 0 đến L. Một số Goombas bắt đầu ở các vị trí số nguyên riêng biệt bên trong đoạn này. Mỗi Goomba có một hướng ban đầu, di chuyển sang trái hoặc phải và tất cả chúng đều di chuyển với cùng tốc độ không đổi một đơn vị mỗi giây. 

Chuyển động có ba quy tắc tương tác. Goomba liên tục di chuyển theo hướng hiện tại của nó. Nếu nó chạm đến một trong hai điểm cuối của đoạn, nó sẽ rơi ra ngay lập tức và bị loại bỏ. Nếu hai Goombas va chạm vào cùng một vị trí, cả hai ngay lập tức đảo hướng và tiếp tục chuyển động với cùng tốc độ. 

Nhiệm vụ không phải là mô phỏng các Goomba riêng lẻ mà là tính toán thời điểm Goomba cuối cùng rời khỏi phân đoạn, nghĩa là hệ thống trở nên trống rỗng. 

Khó khăn chính đến từ quy tắc va chạm. Một cách đọc ngây thơ gợi ý rằng chúng ta phải mô phỏng các tương tác theo cặp, vì các va chạm làm thay đổi hướng và có khả năng tạo ra các chuỗi tương tác trong tương lai. Cách giải thích đó nhanh chóng trở nên không khả thi về mặt tính toán. 

Các giới hạn cho thấy lên tới khoảng 10⁴ Goombas và L rất lớn lên tới 10¹⁶. Bất kỳ giải pháp nào cố gắng mô phỏng theo từng sự kiện hoặc theo dõi tương tác theo cặp sẽ có hành vi O(G²) trong các trường hợp dày đặc vốn đã ở ranh giới và quan trọng hơn là sẽ yêu cầu quản lý sự kiện cẩn thận, điều này không cần thiết đối với số lượng cuối cùng được yêu cầu. 

Một trường hợp phức tạp phát sinh từ các cấu hình đối xứng trong đó nhiều Goombas gặp nhau tại cùng một điểm. Ví dụ: nếu một số Goombas hội tụ vào một vị trí cùng một lúc, tất cả chúng sẽ chuyển hướng đồng thời. Một trình mô phỏng đơn giản có thể xử lý các xung đột theo cặp theo thứ tự tùy ý và phân chia các sự kiện đồng thời không chính xác. 

Một trường hợp khác là khi hai Goombas đạt đến điểm cuối cùng lúc chúng va chạm với một Goomba khác. Tùy thuộc vào thứ tự sự kiện, một mô phỏng đơn giản có thể cho phép Goomba tồn tại hoặc biến mất quá sớm một cách không chính xác, mặc dù tất cả các tương tác đều xảy ra cùng lúc về mặt vật lý. 

## Phương pháp tiếp cận 

Quan sát trọng tâm là quy tắc va chạm không thực sự quan trọng vào thời điểm đó cho đến khi tất cả Goombas biến mất. Khi hai Goomba va chạm và đảo ngược hướng, điều này tương đương với việc chúng đi xuyên qua nhau nếu chúng ta chỉ quan tâm đến vị trí và thời gian biến mất. 

Sự tương đương này có tác dụng vì tất cả các Goomba đều có tốc độ và hành vi đối xứng giống hệt nhau. Nếu chúng ta tưởng tượng Goombas như những hạt không thể phân biệt được, xuyên qua nhau thay vì nảy lên, thì quỹ đạo của chúng trở thành những đường thẳng độc lập. Mỗi Goomba sau đó chỉ cần di chuyển sang trái hoặc phải cho đến khi đạt đến điểm cuối. 

Theo sự chuyển đổi này, các va chạm chỉ hoán đổi danh tính chứ không trao đổi quỹ đạo. Vì chúng tôi chỉ quan tâm đến thời điểm nền tảng trống rỗng nên danh tính không liên quan. 

Vì vậy, thay vì mô phỏng các tương tác, chúng tôi giảm vấn đề xuống việc tính toán, đối với mỗi Goomba, mất bao lâu để đạt đến điểm cuối theo hướng ban đầu của nó. Câu trả lời là thời gian thoát tối đa của từng cá nhân này. 

Cách tiếp cận bạo lực sẽ mô phỏng rõ ràng chuyển động theo từng bước thời gian nhỏ hoặc xử lý các sự kiện va chạm trong hàng đợi ưu tiên. Mỗi vụ va chạm sẽ yêu cầu cập nhật vị trí và có thể sắp xếp lại các sự kiện trong tương lai. Với G lên tới 10⁴, mật độ tương tác trong trường hợp xấu nhất có thể đạt tới O(G²), khiến phương pháp này trở nên quá chậm và phức tạp không cần thiết. 

Cách tiếp cận tối ưu thay thế logic tương tác bằng một phép biến đổi duy nhất: bỏ qua va chạm và coi Goombas như những kẻ đi bộ độc lập. Câu trả lời trở thành mức tối đa đơn giản trên khoảng cách tuyến tính đến ranh giới gần nhất dựa trên hướng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(G2 log G) | O(G) | Quá chậm | 
| Quỹ đạo độc lập | O(G) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc độ dài L và số lượng Goombas G. Đồng thời đọc vị trí p và hướng d của từng Goomba. 
2. Đối với mỗi Goomba, hãy xác định thời gian thoát của nó nếu nó di chuyển một mình. Nếu nó di chuyển sang trái, thời gian thoát của nó là p giây vì nó đạt đến vị trí 0 sau p bước. Nếu nó chuyển động sang phải thì thời gian thoát ra của nó là L − p giây vì nó đạt tới L sau thời gian đó. 
3. Giữ mức hoạt động tối đa trong tất cả thời gian thoát được tính toán. 
4. Sau khi xử lý tất cả Goombas, xuất giá trị tối đa. 

Lý do đằng sau bước 2 là sự đơn giản hóa chính. Khi va chạm được bỏ qua thông qua tính tương đương, mỗi Goomba sẽ trở thành một bài toán chuyển động thẳng với mục tiêu biên cố định. 

### Tại sao nó hoạt động 

Điều bất biến là các va chạm không làm thay đổi nhiều vị trí của Goomba theo thời gian mà chỉ thay đổi danh tính của chúng. Mọi va chạm có thể được hiểu là hai Goombas đi qua nhau và tiếp tục đi theo đường thẳng. Vì tất cả các Goombas đều giống hệt nhau về tốc độ và hành vi nên việc hoán đổi danh tính không ảnh hưởng đến khi một vị trí trống. Do đó, lần cuối cùng Goomba đạt đến ranh giới chính xác là thời điểm sân ga trở nên trống rỗng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    L, G = map(int, input().split())
    ans = 0

    for _ in range(G):
        p, d = map(int, input().split())
        if d == 0:
            ans = max(ans, p)
        else:
            ans = max(ans, L - p)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai được cố ý tối thiểu vì việc chuyển đổi sẽ loại bỏ mọi sự phức tạp trong tương tác. Điểm tinh tế duy nhất là diễn giải chính xác hướng: chuyển động sang trái tương ứng trực tiếp với khoảng cách từ 0, trong khi chuyển động sang phải tương ứng với khoảng cách từ L. 

Không cần cấu trúc sắp xếp hoặc sự kiện vì mỗi Goomba đóng góp độc lập đến mức tối đa cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 1
2 0
```Chúng tôi tính toán thời gian thoát: 

| Goomba | Vị trí | Hướng | Giờ thoát | 
| --- | --- | --- | --- | 
| 1 | 1 | đúng | 3 - 1 = 2 | 
| 2 | 2 | trái | 2 | 

Tối đa là 2. 

Điều này cho thấy một trường hợp đối xứng trong đó cả hai Goombas độc lập đạt đến ranh giới trong cùng một thời điểm và va chạm không ảnh hưởng đến câu trả lời cuối cùng. 

### Ví dụ 2 

đầu vào:```
5 2
1 0
2 1
```| Goomba | Vị trí | Hướng | Giờ thoát | 
| --- | --- | --- | --- | 
| 1 | 1 | trái | 1 | 
| 2 | 2 | đúng | 3 | 

Tối đa là 3. 

Trường hợp này chứng minh rằng mặc dù các Goombas di chuyển về phía nhau và va chạm, vụ va chạm không liên quan đến lần cuối cùng, vì việc hoán đổi danh tính sẽ bảo toàn thời gian thoát ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(G) | Mỗi Goomba đóng góp một phép tính theo thời gian không đổi | 
| Không gian | O(1) | Chỉ lưu trữ mức tối đa đang chạy | 

Giải pháp này phù hợp một cách thoải mái với các ràng buộc vì G tối đa là 10⁴ và tính toán là tuyến tính với chi phí không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve  # assume function extracted
    return solve()

# sample 1
assert run("3 2\n1 1\n2 0\n") == "2"

# sample 2
assert run("5 2\n1 0\n2 1\n") == "3"

# minimum size
assert run("10 1\n5 0\n") == "5"

# all moving right
assert run("10 3\n1 1\n2 1\n3 1\n") == "9"

# all moving left
assert run("10 3\n7 0\n8 0\n9 0\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 1 / 5 0 | 5 | lối ra ranh giới duy nhất | 
| được rồi máy động lực | 9 | khoảng cách tối đa tới ranh giới bên phải | 
| tất cả các máy động lực bên trái | 9 | hành vi biên trái đối xứng | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi tất cả Goombas di chuyển theo cùng một hướng. Ví dụ: nếu tất cả đều chuyển động sang phải, hệ thống hoạt động như các hạt độc lập hướng về L và hạt cuối cùng thoát ra ở khoảng cách tối đa tính từ điểm cuối bên phải. Việc chuyển đổi vẫn được giữ vì không có tương tác nào thay đổi thứ tự hoặc thời gian thoát. 

Một trường hợp góc khác là khi nhiều Goombas va chạm vào cùng một thời điểm và cùng một thời điểm. Ví dụ: các vị trí đối xứng như vị trí 1 và L−1 di chuyển về phía nhau sẽ gây ra va chạm ở điểm giữa. Trong mô hình quỹ đạo độc lập, chúng chỉ đơn giản đi qua nhau. Thời gian thoát không thay đổi và mức tối đa vẫn chỉ được xác định bởi khoảng cách ranh giới. 

Cuối cùng, các trường hợp Goomba bắt đầu rất gần điểm cuối, chẳng hạn như p = 1 hoặc p = L−1, hãy xác nhận rằng giải pháp xử lý chính xác thời gian di chuyển tối thiểu. Những Goombas này có thể tương tác ngay lập tức nhưng thời gian thoát ranh giới vẫn được ghi lại chính xác là 1.
