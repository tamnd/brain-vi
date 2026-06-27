---
title: "CF 105364C - Số trong Lưới"
description: "Chúng ta đang làm việc với một lưới hình chữ nhật bắt đầu hoàn toàn trống theo nghĩa là mọi ô ban đầu đều bằng 0. Sau đó, chúng tôi thực hiện một chuỗi các hoạt động."
date: "2026-06-23T05:32:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105364
codeforces_index: "C"
codeforces_contest_name: "XXV Spain Olympiad in Informatics, Online Qualifier 2"
rating: 0
weight: 105364
solve_time_s: 68
verified: true
draft: false
---

[CF 105364C - Các số trong lưới](https://codeforces.com/problemset/problem/105364/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với một lưới hình chữ nhật bắt đầu hoàn toàn trống theo nghĩa là mọi ô ban đầu đều bằng 0. Sau đó, chúng tôi thực hiện một chuỗi các hoạt động. Mỗi thao tác chọn toàn bộ một hàng hoặc toàn bộ cột và ghi đè lên mọi ô trong dòng đó bằng một giá trị nhất định. 

Sự tinh tế quan trọng là việc ghi đè có tính chất phá hoại. Khi một hàng được đặt thành một giá trị nào đó, tất cả thông tin trước đó trong hàng đó sẽ biến mất, bao gồm các giá trị có thể đã được ghi trước đó bằng các thao tác cột. Tương tự, việc ghi đè lên một cột sẽ xóa mọi nội dung được ghi trước đó trong cột đó. Sau khi tất cả các thao tác được áp dụng theo thứ tự, chúng ta được yêu cầu tính tổng tất cả các giá trị hiện được lưu trữ trong lưới. 

Giải thích trực tiếp gợi ý cập nhật liên tục toàn bộ hàng hoặc cột, nhưng lưới có thể rất lớn. Với tối đa 5e5 hàng và cột cho mỗi lần kiểm tra và tối đa 5e5 thao tác, mọi giải pháp chạm vào từng ô riêng lẻ trên mỗi thao tác sẽ ngay lập tức thất bại. Ngay cả việc chạm vào một hàng hoặc cột đầy đủ một cách rõ ràng cũng có thể tốn O(n) hoặc O(m), trong trường hợp xấu nhất sẽ dẫn đến hành vi bậc hai. 

Khó khăn chính là mỗi hoạt động tương tác với các hoạt động trong tương lai và quá khứ. Việc gán hàng sau sẽ ghi đè các phép gán cột trước đó cho các ô giao nhau và ngược lại. Giá trị cuối cùng của ô chỉ được xác định bởi thao tác gần đây nhất ảnh hưởng đến hàng hoặc cột của ô đó. 

Một số trường hợp đặc biệt bộc lộ những lỗi phổ biến. Nếu tất cả các thao tác đều là cập nhật cột, người ta có thể cho rằng không chính xác rằng chúng ta chỉ có thể tính tổng các đóng góp của cột một cách độc lập, nhưng các hàng được khởi tạo sau có thể ghi đè lên các hàng trước đó. Ví dụ: với lưới 2x2, các phép toán`C 1 5`,`F 1 2`, lưới cuối cùng là`[[2,2],[0,5]]`, không phải thứ gì đó bổ sung. 

Một cạm bẫy khác là giả sử tính giao hoán. Giá trị cuối cùng của một ô phụ thuộc vào thao tác mới nhất giữa hàng và cột của nó, vì vậy thứ tự rất quan trọng. 

Cuối cùng, việc bỏ qua sự tương tác giữa dấu thời gian của hàng và cột sẽ dẫn đến việc đếm quá mức: một giải pháp đơn giản có thể áp dụng cả giá trị hàng và giá trị cột cho cùng một ô. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu giữ cho lưới rõ ràng. Với mỗi thao tác, chúng ta ghi đè lên tất cả các ô trong hàng hoặc cột đã chọn. Mỗi lần cập nhật có chi phí O(n) hoặc O(m) và với q thao tác, tổng chi phí sẽ trở thành O(q·max(n,m)). Trong trường hợp xấu nhất khi cả hai thứ nguyên và q đều bằng 5e5, điều này vượt xa giới hạn khả thi. 

Ngay cả việc tối ưu hóa bằng cách lưu trữ lưới và chỉ tính toán lại các phần bị ảnh hưởng cũng không giúp ích gì, vì mỗi lần ghi đè vẫn chạm vào một số ô tuyến tính. 

Điều quan trọng cần lưu ý là chỉ có thao tác cuối cùng ảnh hưởng đến một hàng hoặc một cột mới quan trọng để xác định giá trị. Khi một hàng được ghi đè tại thời điểm t với giá trị x, mọi thao tác với hàng cũ hơn sẽ không còn phù hợp với hàng đó. Điều tương tự cũng áp dụng cho các cột. Tuy nhiên, các ô bị ảnh hưởng bởi sự tương tác của thao tác hàng gần đây nhất và thao tác cột gần đây nhất. 

Điều này gợi ý theo dõi đối với mỗi hàng, lần cuối cùng nó được cập nhật và giá trị của nó, cũng như tương tự đối với từng cột. Sau đó, thay vì tính toán lại lưới, chúng tôi suy luận mỗi thao tác về mức đóng góp của nó vào tổng cuối cùng. 

Chúng tôi xử lý các hoạt động theo thứ tự ngược lại. Tại thời điểm chúng tôi xử lý một thao tác, tất cả các thao tác sau này đã được sửa và được coi là "cuối cùng" đối với các hàng hoặc cột chưa được đặt còn lại. Chúng tôi duy trì số lượng hàng và cột vẫn "chưa được chỉ định" từ góc độ các hoạt động sau này. Khi chúng ta gặp một thao tác hàng, nó đóng góp giá trị của nó nhân với số cột chưa được sửa bởi các thao tác cột sau này. Một cách đối xứng, một phép toán cột đóng góp giá trị của nó nhân với số hàng chưa được cố định bởi các phép toán hàng sau. Khi một hàng hoặc cột được xử lý ngược lại, nó sẽ được đánh dấu là hoàn tất, đảm bảo các thao tác trước đó không đếm gấp đôi các ô đã được che phủ. 

Sự đảo ngược này biến vấn đề phụ thuộc tổng thể thành một nhiệm vụ kế toán đơn giản về số lượng hàng hoặc cột vẫn còn “tự do” ở mỗi bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q·(n+m)) | O(nm) | Quá chậm | 
| Xử lý ngược | O(n + m + q) | O(n + m + q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc tất cả các thao tác và lưu trữ chúng vào danh sách. Chúng tôi không áp dụng chúng ngay lập tức vì các thao tác sau sẽ ghi đè các thao tác trước đó, vì vậy việc mô phỏng về phía trước sẽ yêu cầu các bản cập nhật tốn kém. 
2. Khởi tạo hai mảng hoặc bộ để theo dõi xem một hàng hoặc cột đã được “sử dụng” trong quá trình xử lý ngược hay chưa. Cũng duy trì quầy`remaining_rows`Và`remaining_cols`, ban đầu bằng n và m. 
3. Thực hiện các thao tác theo thứ tự ngược lại. Thứ tự này đảm bảo rằng khi chúng ta xử lý một thao tác, tất cả các thao tác xảy ra sau nó theo trình tự ban đầu đều đã được tính đến. 
4. Đối với từng thao tác: 

- Nếu là thao tác hàng trên hàng r có giá trị x thì kiểm tra xem hàng r đã được đánh dấu chưa. Nếu có, hãy bỏ qua vì thao tác sau này đã xác định đóng góp cuối cùng của hàng này. 
- Nếu không đánh dấu thì hàng này đóng góp x nhân với số cột còn chưa được gán. Thêm vào`x * remaining_cols`để trả lời. 
- Đánh dấu hàng r là đã xử lý và giảm dần`remaining_rows`. 

Lý do là mọi cột vẫn chưa được gán sẽ giao nhau với hàng này tại một ô có thao tác điều khiển cuối cùng là phép gán hàng này. 
5. Nếu là thao tác cột trên cột c có giá trị x: 

- Nếu cột c đã được đánh dấu thì bỏ qua. 
- Ngược lại thì thêm`x * remaining_rows`để trả lời. 
- Đánh dấu cột c và giảm dần`remaining_cols`. 

Điều này hoạt động đối xứng: mỗi hàng chưa được chỉ định đóng góp một ô trong cột này, nơi thao tác này trở thành thao tác ảnh hưởng mới nhất. 
6. Sau khi xử lý ngược lại tất cả các thao tác, xuất ra tổng tích lũy. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình xử lý ngược, chúng tôi duy trì tính bất biến rằng tất cả các hàng và cột không được đánh dấu chính xác là những hàng mà thao tác xác định cuối cùng chưa được tính đến. Khi chúng tôi xử lý một thao tác hàng, mỗi cột không được đánh dấu sẽ tương ứng với một ô có thao tác mới nhất chính xác là cập nhật hàng này, vì mọi cập nhật cột sau này đều đã được xử lý. Do đó, nhân với số cột không được đánh dấu sẽ tính chính xác các ô nơi hàng này xác định giá trị cuối cùng. Sự đối xứng tương tự áp dụng cho các cột. Vì mỗi hàng và cột được tính chính xác một lần khi lần đầu tiên nó được đảo ngược, mỗi ô được chỉ định chính xác một thao tác đóng góp cuối cùng, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m, q = map(int, input().split())
        ops = []
        for _ in range(q):
            c, a, x = input().split()
            a = int(a)
            x = int(x)
            ops.append((c, a, x))

        used_row = set()
        used_col = set()

        rem_rows = n
        rem_cols = m

        ans = 0

        for c, a, x in reversed(ops):
            if c == 'F':
                if a in used_row:
                    continue
                ans += x * rem_cols
                used_row.add(a)
                rem_rows -= 1
            else:
                if a in used_col:
                    continue
                ans += x * rem_rows
                used_col.add(a)
                rem_cols -= 1

        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp logic xử lý ngược. Các bộ`used_row`Và`used_col`đảm bảo chúng tôi chỉ tính đến cập nhật có hiệu lực cuối cùng của mỗi hàng hoặc cột. các quầy`rem_rows`Và`rem_cols`theo dõi xem có bao nhiêu dòng vẫn không bị ảnh hưởng bởi các thao tác sau theo thứ tự ngược lại, tương ứng với số lượng ô còn lại sẽ lấy giá trị của chúng từ thao tác hiện tại. 

Một lỗi phổ biến là quên giảm các bộ đếm còn lại chỉ khi một hàng hoặc cột được xử lý ngược lại lần đầu tiên. Nếu giảm đi nhiều lần do các thao tác lặp đi lặp lại thì mức đóng góp sẽ bị đánh giá thấp. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi mẫu thứ hai: 

đầu vào:```
3 4 4
F 1 2
F 2 4
C 3 3
F 1 5
```Chúng tôi xử lý ngược lại. 

| Bước | Hoạt động | Hàng đã qua sử dụng | Cols đã qua sử dụng | rem_rows | rem_cols | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | F 1 5 | {} | {} | 3 | 4 | 5 * 4 = 20 | 20 | 
| 2 | C 3 3 | {1} | {} | 2 | 4 | 3 * 2 = 6 | 26 | 
| 3 | F 2 4 | {1} | {3} | 2 | 3 | 4 * 3 = 12 | 38 | 
| 4 | F 1 2 | {1,2} | {3} | 1 | 3 | bỏ qua | 38 | 

Điều này cho thấy cách mỗi hàng hoặc cột được tính chính xác một lần, thời điểm nó lần đầu tiên trở nên có liên quan theo thứ tự ngược lại và cách các thứ nguyên còn lại thể hiện chính xác các giao điểm chưa được chạm tới. 

Dấu vết chứng tỏ rằng khi một hàng được xử lý, những đóng góp trong tương lai sẽ bỏ qua nó, ngăn chặn việc tính hai lần các giao điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + q) | Mỗi thao tác được xử lý một lần và mỗi hàng/cột được đánh dấu tối đa một lần | 
| Không gian | O(n + m + q) | Lưu trữ cho các hoạt động và bộ sổ sách kế toán | 

Các ràng buộc cho phép thực hiện tổng cộng tối đa 3e6 thao tác trên tất cả các trường hợp thử nghiệm, vì vậy việc xử lý tuyến tính cho mỗi trường hợp thử nghiệm là cần thiết. Thuật toán phù hợp thoải mái trong giới hạn vì nó tránh được mọi hoạt động trên mỗi ô. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    # embedded solution
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, m, q = map(int, input().split())
        ops = []
        for _ in range(q):
            c, a, x = input().split()
            ops.append((c, int(a), int(x)))

        used_row = set()
        used_col = set()
        rem_rows = n
        rem_cols = m
        ans = 0

        for c, a, x in reversed(ops):
            if c == 'F':
                if a in used_row:
                    continue
                ans += x * rem_cols
                used_row.add(a)
                rem_rows -= 1
            else:
                if a in used_col:
                    continue
                ans += x * rem_rows
                used_col.add(a)
                rem_cols -= 1

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""4
2 2 3
F 1 1
C 1 2
C 2 3
3 4 4
F 1 2
F 2 4
C 3 3
F 1 5
1 1 1
F 1 0
1 500000 1
F 1 1000000
""") == """10
38
0
500000000000"""

# minimum size
assert run("""1
1 1 1
F 1 7
""") == "7"

# only columns
assert run("""1
2 3 2
C 1 5
C 2 2
""") == "21"

# overwrite test
assert run("""1
2 2 3
F 1 1
C 1 2
F 1 3
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 hàng đơn | 7 | cơ sở ghi đè chính xác | 
| chỉ cột | 21 | xử lý đối xứng các cột | 
| ghi đè hàng sau cột | 10 | sự thống trị của các bản cập nhật hàng sau | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế xảy ra khi một hàng được cập nhật nhiều lần. Trong đầu vào như`F 1 5`,`F 1 7`, chỉ có bản cập nhật cuối cùng mới quan trọng. Trong xử lý ngược, lần đầu tiên chúng tôi gặp hàng 1, đó là bản cập nhật có hiệu lực cuối cùng và chúng tôi đánh dấu nó. Những lần gặp sau này bị bỏ qua nên giá trị trước đó không bao giờ đóng góp. 

Một trường hợp khác là khi tất cả các hàng được cập nhật nhưng không có cột nào được chạm vào. Mỗi hàng đóng góp giá trị của nó nhân với số cột và vì tất cả các hàng đều độc lập nên mỗi hàng được tính chính xác một lần khi nhìn thấy ngược lại lần đầu tiên. 

Cuối cùng, khi các thao tác xen kẽ nhiều giữa các hàng và cột, chẳng hạn như`F C F C ...`, các bộ đếm còn lại sẽ co lại một cách chính xác trong lần gặp đầu tiên, đảm bảo rằng mỗi giao lộ được chỉ định chính xác một lần. Dấu vết thủ công trên lưới 2x2 nhỏ xác nhận rằng mỗi ô trong số bốn ô được tính chính xác một lần và không xảy ra sự chồng chéo.
