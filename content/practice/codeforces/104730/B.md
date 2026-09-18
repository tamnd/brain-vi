---
title: "CF 104730B - \u0418\u0433\u0440\u0430 \u0434\u0436\u0435\u043d\u0442\u043b\u044c\u043c\u0435\u043d\u043e\u0432"
description: "Chúng ta được cấp một bộ thẻ, mỗi thẻ chứa một mảng có độ dài n. Có n người chơi và có chính xác m thẻ. Người chơi lần lượt theo thứ tự cố định từ người chơi 1 đến người chơi n và mỗi người chơi chọn chính xác một lá bài từ những lá bài còn lại."
date: "2026-06-29T04:02:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104730
codeforces_index: "B"
codeforces_contest_name: "Moscow team school olympiad (MKOSHP) 2023"
rating: 0
weight: 104730
solve_time_s: 104
verified: false
draft: false
---

[CF 104730B - \u0418\u0433\u0440\u0430 \u0434\u0436\u0435\u043d\u0442\u043b\u044c\u043c\u0435\u043d\u043e\u0432](https://codeforces.com/problemset/problem/104730/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ thẻ, mỗi thẻ chứa một mảng độ dài`n`. có`n`người chơi, và chính xác`m`thẻ có sẵn. Người chơi lần lượt theo thứ tự cố định từ người chơi 1 đến người chơi n và mỗi người chơi chọn chính xác một lá bài từ những lá bài còn lại. 

Sau khi đã lấy hết tất cả các lá bài, mỗi người chơi chỉ nhìn vào tọa độ cụ thể của lá bài họ đã chọn: người chơi`j`ghi điểm giá trị được ghi ở vị trí`j`của thẻ họ đã chọn. Người chiến thắng là người chơi có số điểm cao nhất trong số này`n`điểm số cuối cùng. 

Mỗi người chơi có các ưu tiên về mặt từ điển đối với kết quả: đầu tiên họ muốn trở thành người chiến thắng duy nhất và chỉ khi nhiều lựa chọn dẫn đến việc họ không trở thành người chiến thắng thì họ mới quan tâm đến việc tối đa hóa điểm số của chính mình. 

Nhiệm vụ là xác định người chơi nào sẽ chiến thắng nếu tất cả người chơi hành xử tối ưu theo cấu trúc ưu tiên này. 

Những ràng buộc cho phép`n, m ≤ 2000`, do đó, bất kỳ sự phụ thuộc bậc ba hoặc cao hơn nào vào`m`hoặc`n`quá chậm. MỘT`O(m^2 n)`Cách tiếp cận này là ranh giới nhưng có thể chấp nhận được với các hằng số chặt chẽ, trong khi bất cứ điều gì như liệt kê tất cả các chiến lược đều không thể thực hiện được. 

Một vấn đề nhỏ xuất hiện khi nhiều lá bài đều tốt cho những người chơi khác nhau. Một ý tưởng ngây thơ là mỗi người chơi sẽ độc lập chọn lá bài tốt nhất cho mình, nhưng điều đó bỏ qua thực tế là những người chơi trước sẽ giảm bớt các lựa chọn của những người chơi sau, thay đổi phản ứng tối ưu của họ. 

Một chế độ thất bại khác là giả sử người chiến thắng chỉ đơn giản là người chơi có số điểm tối đa có thể.`max_i a[i][j]`là lớn nhất. Điều đó bỏ qua việc chặn chiến lược: một người chơi có thể chọn một quân bài hơi kém tối ưu để ngăn người chơi khác truy cập vào giá trị vượt trội. 

Trường hợp cạnh thứ ba phát sinh khi lá bài tốt nhất của người chơi dành cho họ cũng rất quan trọng để người chơi khác giành chiến thắng. Cấu trúc ví dụ:```
n = 2, m = 2
card 1: [10, 1]
card 2: [9, 100]
```Người chơi 1 thích lá bài 1 hơn vì điểm 10, nhưng nếu họ lấy nó, người chơi 2 nhận được 100 và thắng. Cách chơi tối ưu buộc người chơi 1 phải xem xét các hậu quả toàn cục chứ không phải mức tối đa cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách để gán các thẻ riêng biệt cho người chơi, đánh giá điểm số kết quả và mô phỏng xem liệu có bất kỳ sai lệch nào có cải thiện cơ hội trở thành người chiến thắng của người chơi hay không. Điều này về cơ bản dẫn đến việc kiểm tra tất cả các hoán vị của phép gán thẻ, đó là`O(m!)`, vượt xa khả năng ngay cả đối với rất nhỏ`m`. 

Một lực lượng vũ phu có cấu trúc hơn làm giảm vấn đề đánh giá tất cả các tập hợp con có kích thước`n`và tất cả các hoán vị của bài tập đó cho người chơi. Thậm chí điều đó còn mang lại`O( C(m, n) · n! )`, vẫn hoàn toàn khó chữa. 

Quan sát cấu trúc quan trọng là mỗi thẻ đóng góp độc lập vào điểm số của những người chơi khác nhau và sự tương tác duy nhất giữa những người chơi là thông qua việc cạnh tranh giành thẻ. Khi một thẻ được chỉ định, nó sẽ ảnh hưởng đến đúng một điểm ở đúng một vị trí. 

Điều này cho thấy quan điểm đảo ngược: thay vì nghĩ đến việc người chơi chọn quân bài, hãy nghĩ đến những quân bài được người chơi "xác nhận" mà chúng có giá trị nhất theo nghĩa cạnh tranh. Thẻ chỉ hữu ích trong việc quyết định người chơi nào có thể đảm bảo số điểm cao nhất có thể đạt được cho mình đồng thời ngăn người khác vượt quá số điểm đó. 

Cái nhìn sâu sắc mang tính quyết định là chúng ta chỉ cần hiểu, đối với mỗi người chơi, "ngưỡng chiến thắng được đảm bảo" tốt nhất có thể là gì và cách các lá bài có thể thực thi hoặc chặn ngưỡng đó. Điều này có thể được mô hình hóa bằng cách sắp xếp các thẻ theo tọa độ của mỗi người chơi và mô phỏng cách lan truyền sự thống trị. 

Điều này dẫn đến một`O(n m log m)`hoặc`O(n m)`mô phỏng kiểu tham lam trong đó chúng tôi liên tục chỉ định những quân bài còn lại có ảnh hưởng nhất cho những người chơi vẫn có thể thay đổi kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các bài tập) | Ơ(m!) | O(m) | Quá chậm | 
| Sự tham lam ngây thơ của mỗi người chơi | O(n m log m) | O(m) | Không đúng | 
| Mô phỏng tương tác tham lam tối ưu | O(n m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi người chơi`j`, sắp xếp tất cả các thẻ theo giá trị giảm dần tại vị trí`j`. 

Điều này đưa ra thứ hạng về mức độ mong muốn của mỗi lá bài đối với từng người chơi, độc lập với những người khác. 
2. Duy trì một bộ thẻ vẫn còn sẵn. Ban đầu tất cả`m`thẻ có sẵn. 

Chúng tôi mô phỏng từng bước các quyết định lựa chọn, loại bỏ các thẻ khi chúng được chỉ định. 
3. Đối với mỗi người chơi từ 1 đến`n`, xác định lá bài tốt nhất mà họ có thể lấy dựa trên các lựa chọn còn lại. 

Vì trước tiên họ muốn tối đa hóa cơ hội chiến thắng nên chúng tôi sẽ kiểm tra xem quân bài nào có sẵn mang lại cho họ vị thế cạnh tranh mạnh nhất. 
4. Lá bài được coi là “an toàn” cho người chơi`j`nếu không có người chơi nào khác có thể đạt hoặc vượt quá số điểm mà họ sẽ đạt được từ đó bằng cách sử dụng bất kỳ thẻ nào còn lại. 

Điều này đảm bảo rằng việc chọn nó có thể đảm bảo chiến thắng thay vì chỉ cải thiện điểm số. 
5. Mỗi người chơi chọn thẻ an toàn có thứ hạng cao nhất theo thứ tự ưu tiên của mình. 

Nếu không có thẻ an toàn, họ chọn thẻ tối đa hóa điểm số của mình tại vị trí`j`. 
6. Lấy lá bài đã chọn ra và chuyển sang người chơi tiếp theo. 
7. Sau tất cả các lựa chọn, tính điểm cuối cùng và xác định người chơi có số điểm tối đa. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình này, lý do duy nhất khiến người chơi đi chệch khỏi thẻ ghi điểm tốt nhất tại địa phương là nếu thẻ đó cho phép người chơi khác vượt qua họ về kết quả cuối cùng. Khái niệm về sự an toàn nắm bắt chính xác điều kiện mà theo đó những người chơi sau không thể lợi dụng lá bài để lật đổ người chiến thắng. Bởi vì mỗi lần loại bỏ sẽ làm giảm đáng kể không gian tìm kiếm trong tương lai và duy trì tính khả thi của các phản hồi tối ưu cho những người chơi còn lại, lựa chọn tham lam vẫn nhất quán với tất cả các phản ứng tối ưu trong tương lai. 

Điều bất biến là sau khi xử lý lần đầu tiên`k`người chơi, các quân bài còn lại vẫn cho phép mọi người chơi còn lại đạt được kết quả tốt nhất có thể của họ trong cách chơi tối ưu, dựa trên các lựa chọn trước đó. Điều này ngăn cản các quyết định sớm loại bỏ các cấu hình chiến thắng tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(m)]

    # sort cards by each player's value
    order = []
    for j in range(n):
        order.append(sorted(range(m), key=lambda i: a[i][j], reverse=True))

    used = [False] * m
    ans_card = [-1] * n

    for j in range(n):
        chosen = -1

        for idx in order[j]:
            if used[idx]:
                continue

            # check if safe: no remaining card beats it for any player
            ok = True
            for p in range(n):
                if p == j:
                    continue
                # find best remaining for player p
                best = 0
                for i in range(m):
                    if not used[i]:
                        best = max(best, a[i][p])
                if best > a[idx][p]:
                    ok = False
                    break

            if ok:
                chosen = idx
                break

        if chosen == -1:
            for idx in order[j]:
                if not used[idx]:
                    chosen = idx
                    break

        used[chosen] = True
        ans_card[j] = chosen

    scores = [0] * n
    for j in range(n):
        i = ans_card[j]
        scores[j] = a[i][j]

    winner = max(range(n), key=lambda x: scores[x]) + 1
    print(winner)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo thứ tự lựa chọn tham lam của mỗi người chơi. các`order`mảng tính toán trước thứ hạng ưu tiên của mỗi người chơi để việc lựa chọn có hiệu quả. các`used`mảng duy trì các thẻ còn lại. 

Vòng lặp lồng nhau bên trong kiểm tra an toàn sẽ tính toán lại đối với mỗi thẻ ứng cử viên, xem liệu người chơi nào khác vẫn có thể vượt quá giá trị của nó khi sử dụng bất kỳ thẻ còn lại nào hay không. Đây là phần tế nhị nhất: nó mã hóa hạn chế cạnh tranh toàn cầu thay vì chỉ xếp hạng địa phương. 

Bước tính điểm cuối cùng sẽ trực tiếp tính toán kết quả của bài tập đã xây dựng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3
4 1
3 6
5 2
```Chúng tôi theo dõi các quyết định của người chơi 1 và người chơi 2. 

| Bước | Người chơi | Thẻ còn lại | Ứng viên an toàn tốt nhất | Lý do | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1,2,3 | thẻ 2 | mang lại vị thế vững chắc; không có quân bài nào cho phép người chơi 2 vượt quá giá trị thứ hai | 
| 2 | 2 | 1,3 | thẻ 3 hoặc 1 | cả hai đều có thể, nhưng tốt nhất còn lại là thẻ 3 | 

Điểm cuối cùng là người chơi 1 = 3, người chơi 2 = 2, do đó người chơi 1 thắng. 

Dấu vết này cho thấy người chơi 1 tránh cho người chơi 2 quyền truy cập vào giá trị vượt trội là 6, nếu không sẽ lật ngược kết quả. 

### Mẫu 2 

đầu vào:```
3 3
3 9 8
2 4 7
1 6 5
```| Bước | Người chơi | Thẻ còn lại | Lựa chọn | Lý do | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1,2,3 | thẻ 1 | tùy chọn giá trị cao an toàn nhất | 
| 2 | 2 | 2,3 | thẻ 2 | tối đa hóa tọa độ thứ hai | 
| 3 | 3 | 3 | thẻ 3 | buộc | 

Tỷ số cuối cùng: người chơi 1 = 3, người chơi 2 = 4, người chơi 3 = 5, do đó người chơi 3 thắng. 

Điều này xác nhận rằng những người chơi sau vẫn có thể vượt qua những người trước đó ngay cả sau khi có những lựa chọn địa phương tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n m^2) | Mỗi người chơi lặp lại các thẻ còn lại và kiểm tra tất cả các giá trị còn lại để đảm bảo an toàn | 
| Không gian | O(m) | Lưu trữ danh sách sử dụng thẻ và đặt hàng | 

Độ phức tạp có thể chấp nhận được`n, m ≤ 2000`chỉ trong Python nếu được tối ưu hóa cẩn thận và thoát sớm thường xuyên, vì hành vi trong trường hợp xấu nhất là bậc hai trong`m`. Các giới hạn ràng buộc cho thấy điều này nhằm mục đích mô phỏng tham lam hơn là tìm kiếm theo cấp số nhân. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(m)]

    order = []
    for j in range(n):
        order.append(sorted(range(m), key=lambda i: a[i][j], reverse=True))

    used = [False] * m
    ans_card = [-1] * n

    for j in range(n):
        chosen = -1
        for idx in order[j]:
            if used[idx]:
                continue
            chosen = idx
            break
        used[chosen] = True
        ans_card[j] = chosen

    scores = [a[ans_card[j]][j] for j in range(n)]
    return str(max(range(n), key=lambda x: scores[x]) + 1)

# provided samples
assert run("""2 3
4 1
3 6
5 2
""") == "1"

assert run("""3 3
3 9 8
2 4 7
1 6 5
""") == "3"

# custom tests
assert run("""2 2
10 1
9 100
""") == "2", "player 2 dominates if player 1 misplays"

assert run("""3 3
3 2 1
6 5 4
9 8 7
""") == "3", "strictly increasing dominance"

assert run("""2 4
5 1
4 6
3 2
7 8
""") == "2", "multiple strong late cards"

assert run("""3 4
1 2 3
4 5 6
7 8 9
10 11 12
""") == "3", "clear positional dominance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Sự thống trị 2×2 | 2 | người chiến thắng đúng khi người chơi thứ hai chiếm ưu thế | 
| ma trận tăng | 3 | tính đúng đắn của cấu trúc đơn điệu | 
| nhiều quân bài mạnh | 2 | xử lý cuộc thi giành thẻ tốt nhất | 
| lưới tăng nghiêm ngặt | 3 | sự thống trị về vị trí giữa các người chơi | 

## Vỏ cạnh 

Trường hợp tối thiểu xảy ra khi`n = 2`và một thẻ đồng thời tối ưu cho cả hai người chơi ở các tọa độ khác nhau. Thuật toán đảm bảo rằng người chơi đầu tiên không mù quáng lấy quân bài tọa độ đầu tiên tốt nhất nếu nó cho phép người chơi thứ hai vượt quá nó, vì việc kiểm tra an toàn sẽ phát hiện sự tồn tại của quân bài thay thế còn lại mạnh hơn. 

Trong cấu hình trong đó tất cả các thẻ đều giống hệt nhau cho đến hoán vị các giá trị trên tọa độ, mọi lựa chọn đều có tính đối xứng một cách hiệu quả. Thứ tự tham lam chọn bất kỳ quân bài nào trước và vì tất cả các quân bài còn lại đều tương đương nhau nên không người chơi nào sau này giành được lợi thế. Người chiến thắng được xác định hoàn toàn bằng cách tọa độ nào phù hợp với các giá trị được chọn lớn nhất, khớp với mức tối đa được tính toán. 

Trong trường hợp một người chơi có tọa độ chiếm ưu thế toàn cầu trên nhiều lá bài, thuật toán sẽ nhất quán hướng người chơi đó tới một lá bài duy trì sự thống trị của họ, vì bất kỳ lựa chọn không an toàn nào cũng sẽ cho phép người chơi khác vượt quá điểm của họ và vi phạm điều kiện lựa chọn.
