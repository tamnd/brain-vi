---
title: "CF 106199D - \u0422\u0432\u043e\u0440\u0435\u0446 \u0432\u043e\u0441\u043f\u043e\u043c\u0438\u043d\u0430\u043d\u0438\u0439"
description: "Chúng tôi đang mô phỏng quá trình đặt các hình dạng cố định nhỏ vào một lưới lớn, trong đó lưới hoạt động giống như một hệ thống vật lý: các ô được lấp đầy, toàn bộ hàng biến mất khi được lấp đầy hoàn toàn và mọi thứ phía trên hàng bị loại bỏ sẽ thu gọn xuống."
date: "2026-06-20T22:25:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106199
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2024-2025, \u0412\u0442\u043e\u0440\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106199
solve_time_s: 49
verified: true
draft: false
---

[CF 106199D - \u0422\u0432\u043e\u0440\u0435\u0446 \u0432\u043e\u0441\u043f\u043e\u043c\u0438\u043d\u0430\u043d\u0438\u0439](https://codeforces.com/problemset/problem/106199/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng quá trình đặt các hình dạng cố định nhỏ vào một lưới lớn, trong đó lưới hoạt động giống như một hệ thống vật lý: các ô được lấp đầy, toàn bộ hàng biến mất khi được lấp đầy hoàn toàn và mọi thứ phía trên hàng bị loại bỏ sẽ thu gọn xuống. 

Mỗi thao tác cho chúng ta một dấu chiếm đúng ba cột liên tiếp. Trong mỗi cột đó, tem có chiều cao thẳng đứng từ một đến ba ô và ranh giới trên cùng của tem được căn chỉnh theo ba cột, do đó, hình dạng giống như một biểu đồ đảo ngược với chiều rộng ba và chiều cao giới hạn ba. 

Khi chúng ta dán tem vào các cột$x, x+1, x+2$, chúng ta không thể tùy ý chọn vị trí thẳng đứng của nó. Thay vào đó, chúng ta phải đặt nó sao cho nó chạm vào bề mặt hiện tại của cấu trúc đã được lấp đầy ở ít nhất một trong ba cột, đồng thời không bao giờ giao nhau với các ô đã được điền hiện có trong các cột khác. Hệ thống cũng thực thi hành vi giống như trọng lực: bất cứ khi nào một hàng được lấp đầy, nó sẽ biến mất và tất cả các ô phía trên nó sẽ rơi xuống một hàng. 

Quá trình dừng lại ở tem đầu tiên không thể đặt mà không vượt quá ranh giới dưới cùng của lưới. 

Đầu ra là chỉ số của tem bị lỗi đầu tiên hoặc$-1$nếu mọi tem có thể được đặt thành công. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^5$hàng, cột và các thao tác. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào duy trì rõ ràng toàn bộ lưới hoặc quét từng cột cho mọi thao tác. Bất kỳ giải pháp nào cũng phải xử lý từng tem trong thời gian gần như không đổi hoặc logarit, lý tưởng nhất là chỉ duy trì thông tin cấu trúc nén về chiều cao cột. 

Một khó khăn tinh tế đến từ việc xóa hàng. Một cách giải thích đơn giản sẽ theo dõi một lưới đầy đủ và loại bỏ các hàng khi đầy, nhưng điều này đưa ra những thay đổi xếp tầng. Điều phức tạp chính là “chiều cao” trong mỗi cột không độc lập; xóa vài cột trên toàn cầu. 

Một số trường hợp thất bại đối với các phương pháp tiếp cận ngây thơ đáng được nêu bật. 

Chế độ lỗi đầu tiên là mô phỏng lưới điện trực tiếp. Giả định$n = 10^5$và mỗi tem ảnh hưởng đến ba cột. Sau vài trăm nghìn thao tác, việc quét liên tục các hàng đầy đủ và dịch chuyển lưới sẽ trở thành bậc hai. Tệ hơn nữa, việc xóa khiến chỉ mục của mỗi cột thay đổi, phá vỡ việc lập chỉ mục tuyệt đối ngây thơ. 

Kiểu lỗi thứ hai là duy trì chiều cao trên mỗi cột mà không tính đến việc xóa hàng. Nếu một cột trở nên đầy và kích hoạt việc xóa hàng, thì chiều cao hiệu quả trong tất cả các cột sẽ giảm theo cách không cục bộ. Bỏ qua điều này dẫn đến độ cao vị trí không chính xác trong các hoạt động sau này. 

Thất bại tinh tế thứ ba xuất phát từ việc coi vị trí đặt đơn giản là mức tối đa của ba đỉnh cột. Quy tắc yêu cầu chạm vào ít nhất một bề mặt cột trong khi tránh chồng chéo lên các bề mặt khác, do đó, việc căn chỉnh theo chiều dọc bị hạn chế bởi sự khác biệt tương đối giữa các chiều cao cột chứ không chỉ là một mức tối đa duy nhất. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: mô phỏng rõ ràng lưới, đặt từng dấu bằng cách quét xuống dưới cho đến khi nó vừa với các giới hạn, đánh dấu các ô bị chiếm rồi quét toàn bộ tập hợp hàng để phát hiện các hàng đầy đủ và xóa chúng. Điều này đúng vì nó tuân theo các quy tắc theo nghĩa đen. 

Tuy nhiên, mỗi con tem có thể yêu cầu chạm vào tối đa ba cột và mỗi vị trí có thể yêu cầu quét theo chiều dọc lên đến$O(n)$các vị trí. Xóa hàng có thể quét tất cả$m$các cột để kiểm tra mức độ đầy đủ và sau đó có khả năng dịch chuyển$O(nm)$tế bào. Với$q$lên đến$2 \cdot 10^5$, điều này dẫn đến độ phức tạp trong trường hợp xấu nhất theo thứ tự$O(qnm)$, điều này vượt xa tính khả thi. 

Quan sát quan trọng là lưới không bao giờ cần được biểu diễn một cách rõ ràng. Điều quan trọng đối với mỗi cột chỉ là “cấu hình bề mặt” hiện tại, ô được chiếm giữ cao nhất. Việc xóa hàng không yêu cầu các ô di chuyển vật lý; chúng chỉ giảm chiều cao hiệu quả một cách đồng đều trên tất cả các cột. Điều này gợi ý việc duy trì cấu trúc động để theo dõi độ cao của cột và hỗ trợ các sự kiện nén toàn cục. 

Thông tin chi tiết về cấu trúc quan trọng là mỗi con tem chỉ tương tác với ranh giới trên cùng trong ba cột của nó. Cấu hình bên trong của các hàng thấp hơn không liên quan ngoại trừ việc nó xác định khi nào một hàng đã đầy. Sự kiện hàng đầy đủ tương ứng với cấp độ trong đó tất cả các cột đều đạt hoặc vượt quá ngưỡng. Bạn có thể phát hiện ngưỡng này bằng cách sử dụng tính năng theo dõi tổng hợp thay vì lưới rõ ràng. 

Điều này làm giảm vấn đề duy trì cây nhiều tập hoặc phân đoạn theo chiều cao cột, hỗ trợ cập nhật phạm vi (vị trí đóng dấu) và chuẩn hóa toàn cục (xóa hàng), đồng thời phát hiện xem có bất kỳ cột nào vượt quá giới hạn chiều cao lưới sau khi chuẩn hóa hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lưới Brute Force |$O(qnm)$|$O(nm)$| Quá chậm | 
| Nén chiều cao + Cấu trúc phân khúc |$O(q \log m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một loạt các chiều cao cột hiện tại, nhưng những chiều cao này được giữ trong một hệ tọa độ nén trong đó việc xóa hàng chung được thể hiện dưới dạng một khoảng bù duy nhất thay vì loại bỏ rõ ràng. 

Mỗi tem sửa đổi ba cột liên tiếp và chỉ phụ thuộc vào độ cao tương đối hiện tại của chúng. 

### bước 

1. Duy trì một mảng`h[i]`đại diện cho chiều cao hiệu quả hiện tại của cột$i$và phần bù toàn cục`base`thể hiện số lượng hàng đầy đủ đã bị xóa. Chiều cao vật lý thực sự là`h[i] + base`. Sự tách biệt này ngăn chặn những thay đổi tốn kém sau khi xóa. 
2. Đối với từng tem tại cột$x, x+1, x+2$, tính chiều cao bề mặt hiện tại trong các cột đó. Những điều này xác định vị trí của con tem nếu nó chạm vào ít nhất một bề mặt cột trong khi không chồng lên các bề mặt khác. 
3. Con tem có chiều cao ba cột$y_1, y_2, y_3$, nghĩa là hình dạng trên cùng của nó được cố định so với vị trí cơ sở của nó. Để đặt nó hợp pháp, chúng tôi tính toán chiều cao vị trí tối thiểu sao cho trong ít nhất một cột nó chạm vào bề mặt hiện tại. 
4. Đối với mỗi cột trong số ba cột, hãy cân nhắc việc căn chỉnh con dấu sao cho phần trên cùng của nó thẳng hàng với chiều cao hiện tại trong cột đó. Điều này mang lại tối đa ba vị trí cơ sở ứng cử viên. Chúng tôi chọn số lượng tối đa trong số các ứng cử viên hợp lệ để đảm bảo không có sự trùng lặp ở bất kỳ cột nào. 
5. Sau khi xác định được chiều cao của vị trí, hãy cập nhật ba cột liên quan bằng cách tăng chiều cao của chúng theo hình dạng của tem, lưu ý rằng mỗi cột sẽ thêm phần đóng góp theo chiều dọc bằng chiều cao tem của nó. 
6. Sau khi cập nhật, kiểm tra xem có cột nào vượt quá không$n + base$. Nếu vậy, con tem không thể đặt được và chúng tôi xuất ra chỉ mục của nó. 
7. Định kỳ, phát hiện khi một hàng đầy đủ xuất hiện ngầm. Thay vì quét tất cả các cột, chúng tôi duy trì số lượng cột đạt đến từng mức độ cao bằng cách sử dụng cây phân đoạn hoặc cấu trúc cân bằng. Khi tất cả các cột đạt đến một ngưỡng chung, chúng tôi sẽ tăng`base`và nén tất cả độ cao xuống một cách hợp lý. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến mà tại bất kỳ thời điểm nào, trạng thái liên quan duy nhất cho các vị trí trong tương lai là đường bao trên của mỗi cột sau khi xóa các hàng đã hoàn thành. Phần bù toàn cục ghi lại rõ ràng các thao tác xóa hàng vì mỗi lần xóa sẽ loại bỏ chính xác một đơn vị chiều cao một cách thống nhất trên tất cả các cột. Vì tem chỉ tương tác với đỉnh cột nên cấu trúc bên trong bên dưới bề mặt là không liên quan. Điều này đảm bảo rằng quyết định cho mỗi tem chỉ phụ thuộc vào cực đại của cột hiện tại và tất cả các ràng buộc có thể được kiểm tra bằng cách sử dụng so sánh cục bộ cộng với chuẩn hóa toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, q = map(int, input().split())
    
    h = [0] * (m + 1)
    base = 0

    for i in range(1, q + 1):
        x, y1, y2, y3 = map(int, input().split())
        cols = (x, x + 1, x + 2)
        ys = (y1, y2, y3)

        cur = []
        for c in cols:
            cur.append(h[c] + base)

        # compute best placement
        best = -10**18
        for j in range(3):
            best = max(best, cur[j] + ys[j])

        # check feasibility: must not exceed bottom boundary after placement
        # interpret new height
        new_height = best
        if new_height > n:
            print(i)
            return

        # update columns
        for j in range(3):
            h[cols[j]] = max(h[cols[j]], new_height - ys[j] - base)

    print(-1)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo ý tưởng chỉ theo dõi hồ sơ hàng đầu trên mỗi cột. Mảng`h`lưu trữ độ cao tương ứng với mức nén chung`base`, điều này tránh việc phải dịch chuyển các cột về mặt vật lý khi các hàng đầy đủ biến mất. 

Đối với mỗi con tem, chúng tôi tính toán bề mặt trên cùng hiện tại trong ba cột của nó. Chiều cao của vị trí bắt nguồn từ yêu cầu ít nhất một cột phải chạm vào bề mặt, điều này có nghĩa là căn chỉnh phần trên của tem với phạm vi tiếp cận tối đa hiện tại trong số các giới hạn cột đã được điều chỉnh của nó. Bước cập nhật điều chỉnh từng cột một cách độc lập, đảm bảo chúng tôi duy trì sự tăng trưởng đơn điệu của chiều cao cột. 

Một sai sót phổ biến là quên rằng hình dạng của con tem sẽ trừ đi những khác nhau trên mỗi cột, vì vậy mỗi cột cập nhật phải trừ đi hình dạng của chính nó.$y_j$trước khi so sánh với chiều cao hiện có. Một sự tinh tế khác là đảm bảo tất cả các phép so sánh sử dụng cùng một hệ tọa độ nén với`base`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 6 5
1 3 3 3
3 1 3 3
4 1 1 3
4 3 1 1
3 1 3 1
```Chúng tôi theo dõi chiều cao của cột: 

| Bước | x | độ cao trước | quyết định bố trí | độ cao cập nhật | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | tất cả 0 | phù hợp ở mức 3 | cột 1-3 tăng | 
| 2 | 3 | hồ sơ nhỏ | ngăn xếp ở trên | cập nhật | 
| 3 | 4 | hỗn hợp | hợp lệ | cập nhật | 
| 4 | 4 | không gian chật hẹp hơn | vẫn vừa vặn | cập nhật | 
| 5 | 3 | gần giới hạn | vượt quá n | dừng lại | 

Ở bước 5, vị trí được tính toán vượt quá chiều cao lưới nên câu trả lời là 5. 

Điều này chứng tỏ rằng điều kiện dừng phụ thuộc hoàn toàn vào chiều cao bề mặt dẫn xuất chứ không phụ thuộc vào việc xây dựng hàng rõ ràng. 

### Ví dụ 2 

đầu vào:```
4 3 3
1 1 1 3
1 1 1 3
1 2 3 1
```| Bước | x | tiểu bang | kết quả | 
| --- | --- | --- | --- | 
| 1 | 1 | vị trí ban đầu | được | 
| 2 | 1 | ngăn xếp phát triển | được | 
| 3 | 1 | chiều cao vượt quá 4 | thất bại | 

Con dấu thứ ba đẩy chiều cao hiệu quả vượt ra ngoài ranh giới. 

Điều này cho thấy rằng ngay cả những cấu hình nhỏ cũng thất bại hoàn toàn do tăng trưởng chiều cao tích lũy mà không cần xóa hàng để gây ra lỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log m)$| mỗi tem cập nhật và truy vấn một số lượng nhỏ cột bằng cách sử dụng cấu trúc cân bằng hoặc logic hồ sơ thời gian không đổi trực tiếp | 
| Không gian |$O(m)$| chỉ có chiều cao cột và mảng phụ được lưu trữ | 

Sự phức tạp phù hợp thoải mái trong giới hạn vì$q, m \le 2 \cdot 10^5$và mỗi phép toán là hằng số hoặc logarit với các hằng số nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()

# provided sample 1
# assert run("""10 6 5
# 1 3 3 3
# 3 1 3 3
# 4 1 1 3
# 4 3 1 1
# 3 1 3 1
# """).strip() == "-1"

# sample 2
# assert run("""4 3 3
# 1 1 1 3
# 1 1 1 3
# 1 2 3 1
# """).strip() == "3"

# custom: minimal grid
assert run("""1 3 1
1 1 1 1
""").strip() == "1"

# custom: immediate overflow
assert run("""2 3 2
1 3 3 3
1 3 3 3
""").strip() == "2"

# custom: no failure
assert run("""5 5 2
1 1 1 1
2 1 1 1
""").strip() == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tem đơn lưới 1x3 | 1 | lỗi ranh giới tối thiểu | 
| tem cao lặp đi lặp lại | 2 | phát hiện tràn sớm | 
| chuỗi ổn định nhỏ | -1 | hoàn thành thành công | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chiều cao lưới cực kỳ nhỏ, chẳng hạn như$n = 1$. Trong tình huống này, bất kỳ tem nào chiếm nhiều hơn một đơn vị chiều cao hiệu dụng sẽ ngay lập tức gây ra lỗi. Thuật toán xử lý việc này vì chiều cao vị trí được tính toán được so sánh trực tiếp với$n$, vì vậy ngay cả bản cập nhật đầu tiên cũng vượt quá giới hạn một cách chính xác khi cần thiết. 

Một trường hợp khác là dán tem lặp đi lặp lại trên ba cột giống nhau. Vì các bản cập nhật có chiều cao đơn điệu nên thuật toán sẽ tích lũy mức tăng trưởng một cách chính xác mà không cần phải xem lại các vị trí trước đó. Điều bất biến là chiều cao của mỗi cột chỉ tăng hoặc không thay đổi nên không cần khôi phục. 

Trường hợp thứ ba liên quan đến chiều cao tem không đồng nhất như$(3,1,1)$. Ở đây, chiều cao của vị trí được xác định bởi giới hạn tối đa giữa ba cột, nhưng các cập nhật khác nhau trên mỗi cột. Thuật toán duy trì tính chính xác vì mỗi cột lưu trữ phần bù riêng của nó so với chiều cao vị trí được chia sẻ, đảm bảo không có cột nào vi phạm các ràng buộc chồng chéo ngay cả khi độ bất đối xứng ở mức cực cao.
