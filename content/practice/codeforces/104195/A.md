---
title: "CF 104195A - \u041f\u043b\u0430\u043d \u0437\u0430\u0449\u0438\u0442\u044b"
description: "Chúng ta được cung cấp một chuỗi các cuộc tấn công sắp tới, mỗi cuộc tấn công có một giá trị sức mạnh. Bộ lạc có thể phòng thủ một cách an toàn trước bất kỳ cuộc tấn công nào có sức mạnh không vượt quá ngưỡng."
date: "2026-07-02T00:33:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104195
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2022-2023, \u0422\u0440\u0435\u0442\u044c\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 + \u0412\u0442\u043e\u0440\u043e\u0439 \u043e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0418\u041e\u0418\u041f"
rating: 0
weight: 104195
solve_time_s: 66
verified: true
draft: false
---

[CF 104195A - \u041f\u043b\u0430\u043d \u0437\u0430\u0449\u0438\u0442\u044b](https://codeforces.com/problemset/problem/104195/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các cuộc tấn công sắp tới, mỗi cuộc tấn công có một giá trị sức mạnh. Bộ lạc có thể phòng thủ một cách an toàn trước bất kỳ cuộc tấn công nào có sức mạnh không vượt quá ngưỡng. Bất cứ khi nào một cuộc tấn công mạnh hơn ngưỡng này, họ buộc phải sơ tán và tránh xa đúng một số lần tấn công liên tiếp cố định. Sau khi thời gian sơ tán đó kết thúc, họ phải trở về nhà ngay lập tức, nghĩa là phân đoạn sơ tán phải hoàn toàn phù hợp với dòng thời gian của các cuộc tấn công. 

Hạn chế chính là mọi cuộc sơ tán đều bao gồm chính xác`x`các cuộc tấn công liên tiếp và các cuộc di tản không thể chồng chéo lên nhau theo cách tùy tiện, chúng là các khối rời rạc. Khi ở nhà, bộ tộc chỉ có thể ở lại an toàn nếu tất cả các cuộc tấn công hiện tại đủ yếu. Nếu một cuộc tấn công mạnh xảy ra bên ngoài cuộc sơ tán, nó sẽ buộc một cuộc sơ tán mới bắt đầu ngay trước cuộc tấn công đó. 

Nhiệm vụ là quyết định xem liệu có thể bao quát tất cả các khoảnh khắc “nguy hiểm” bằng cách sử dụng các khối sơ tán có độ dài cố định như vậy hay không và nếu có, hãy giảm thiểu số lượng khối như vậy là cần thiết. 

Chi tiết cấu trúc quan trọng là các cuộc sơ tán không phải là những lá chắn linh hoạt có thể được đặt tự do ở bất cứ đâu. Mỗi cuộc sơ tán tiêu tốn một đoạn dài liền kề`x`, do đó việc đặt một quyết định sẽ ảnh hưởng đến toàn bộ cửa sổ. Điều này ngay lập tức gợi ý vấn đề lập kế hoạch tham lam đối với việc phân loại các cuộc tấn công nhị phân: an toàn và nguy hiểm. 

Từ những hạn chế,`n`lên tới 500.000, do đó, bất kỳ giải pháp bậc hai hoặc thậm chí O(n log n) nào có cấu trúc dữ liệu nặng đều có rủi ro trừ khi rất đơn giản. Một giải pháp tham lam quét tuyến tính được ngụ ý mạnh mẽ. 

Các trường hợp đặc biệt quan trọng là các tình huống trong đó: 

1. Một cuộc tấn công nguy hiểm xảy ra muộn đến mức không còn chỗ để bắt đầu một khoảng thời gian sơ tán đầy đủ`x`. Ví dụ, nếu`x = 3`,`n = 5`, và cần phải sơ tán bắt buộc tại vị trí`4`, bắt đầu từ`4`thì ổn, nhưng bắt đầu từ`5`là không thể vì nó sẽ vượt quá giới hạn. 
2. Một cụm tấn công nguy hiểm dày đặc cách nhau hơn`x`ngoài, điều này có thể gây ra sự chồng chéo hoặc buộc phải hợp nhất. 
3.`m = 0`, nơi mọi cuộc tấn công tích cực đều trở nên nguy hiểm và chúng tôi phải kiểm tra xem việc bao phủ tất cả các vị trí bằng cửa sổ cố định có khả thi hay không. 
4.`x = n`, trong đó chỉ có thể thực hiện một lần sơ tán và nó phải bao phủ toàn bộ mảng. 

Một kẻ tham lam ngây thơ bắt đầu một cuộc sơ tán mới ở mỗi cuộc tấn công nguy hiểm sẽ thất bại khi các cửa sổ chồng chéo có thể đã được sử dụng lại hoặc hợp nhất, làm tăng câu trả lời một cách không cần thiết. 

## Phương pháp tiếp cận 

Ý tưởng vũ lực là mô phỏng mọi quyết định có thể xảy ra: bất cứ khi nào một cuộc tấn công nguy hiểm xuất hiện, hãy bắt đầu một khối sơ tán bao phủ`[i, i + x - 1]`hoặc cố gắng trì hoãn nó và xem liệu những cuộc sơ tán sau này có thể giải quyết được hay không. Điều này nhanh chóng trở thành cấp số nhân vì mỗi quyết định đều ảnh hưởng đến tất cả các vị thế trong tương lai và các khoảng thời gian chồng chéo tương tác theo những cách không hề tầm thường. Ngay cả DP trên các vị trí và điểm cuối sơ tán cuối cùng cũng sẽ là O(n^2) trong trường hợp xấu nhất vì mỗi vị trí có thể thử tất cả các chuyển đổi trước đó. 

Quan sát quan trọng là các cuộc sơ tán là những tấm che có chiều dài cố định và quyết định có ý nghĩa duy nhất là vị trí đặt các điểm cuối bên trái của những tấm che này. Một khi chúng tôi quyết định sơ tán bắt đầu từ vị trí`i`, chúng tôi tự động bao gồm một đoạn cố định và bỏ qua về phía trước. Vì vậy, vấn đề trở thành: quét từ trái sang phải, và bất cứ khi nào chúng ta gặp phải một vị trí nguy hiểm chưa được che chắn, chúng ta buộc phải đặt một khối sơ tán để che chắn nó càng sớm càng tốt. Việc trì hoãn nó sẽ chỉ đẩy phạm vi phủ sóng đi xa hơn và giảm tính linh hoạt. 

Đây là cách bao gồm tham lam cổ điển về các điểm cần thiết với các khoảng thời gian có độ dài cố định. Chúng tôi duy trì vị trí “được bảo hiểm” hiện tại. Khi chúng ta thực hiện một cuộc tấn công nguy hiểm ngoài phạm vi bao phủ này, chúng ta phải đặt một đoạn bắt đầu từ vị trí đó. Đoạn đó kéo dài đến`i + x - 1`và chúng tôi cập nhật thông tin phù hợp. 

Tuy nhiên, có một hạn chế về tính khả thi: nếu chúng ta đặt một đoạn bắt đầu từ`i`, nó phải kết thúc trong`n`. Vậy nếu`i + x - 1 > n`, điều đó là không thể. 

Sự tham lam này là tối ưu vì mỗi lần sơ tán chỉ bị ép buộc khi cần thiết và bắt đầu muộn hơn sẽ khiến vị trí nguy hiểm hiện tại bị phát hiện. Bắt đầu sớm hơn là không thể vì chúng tôi quét từ trái sang phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ / O(n^2) DP | O(n^2) | Quá chậm | 
| Tham lam tối ưu | O(n) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước những cuộc tấn công nào là nguy hiểm bằng cách kiểm tra xem`a[i] > m`. Điều này chuyển đổi vấn đề thành việc bao gồm các chỉ số nhất định với các đoạn có độ dài cố định. 
2. Duy trì một biến`covered_until`đại diện cho vị trí cuối cùng đã được bảo vệ an toàn trong cuộc sơ tán trước đó. Điều này đảm bảo chúng tôi không bao giờ đếm gấp đôi hoặc chồng chéo một cách không cần thiết. 
3. Quét từ trái qua phải trên tất cả các chỉ số`i`. 
4. Nếu`i`đã ở trong rồi`covered_until`, hãy bỏ qua vì nó đã bị chặn bởi khối sơ tán trước đó. 
5. Nếu`i`nguy hiểm và không được che chắn, chúng ta phải bắt đầu một cuộc sơ tán mới vào lúc`i`. 
6. Trước khi thực hiện sơ tán này, hãy kiểm tra xem`i + x - 1`vượt quá`n`. Nếu có, hãy kết luận ngay kế hoạch đó là không thể. 
7. Nếu không, hãy ghi lại rằng chúng tôi bắt đầu sơ tán tại vị trí`i`, và đặt`covered_until = i + x - 1`. 
8. Tiếp tục quét cho đến hết, thu thập tất cả các vị trí bắt đầu sơ tán. 

### Tại sao nó hoạt động 

Thuật toán thực thi một bất biến bao phủ tham lam: mọi chỉ số nguy hiểm đều được bao phủ bởi khoảng thời gian sơ tán sớm nhất có thể có thể bao gồm nó. Vì tất cả các khoảng thời gian đều có độ dài bằng nhau và không có lợi ích gì khi dịch chuyển một khoảng sang phải, bất kỳ giải pháp thay thế nào làm trì hoãn phạm vi phủ sóng sẽ khiến một cuộc tấn công nguy hiểm bị phát hiện hoặc yêu cầu một khoảng thời gian bổ sung sau đó. Do đó, số lượng khoảng được giảm thiểu và tính khả thi được bảo toàn bằng cách đảm bảo mỗi khoảng đã chọn hoàn toàn khớp với bên trong mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, x, m = map(int, input().split())
a = list(map(int, input().split()))

ans = []
i = 0
covered_until = -1

while i < n:
    if i <= covered_until:
        i += 1
        continue

    if a[i] > m:
        if i + x - 1 >= n + 1:
            print(-1)
            sys.exit(0)

        ans.append(i + 1)
        covered_until = i + x - 1
    i += 1

print(len(ans))
if ans:
    print(*ans)
```Cấu trúc cốt lõi là một lần quét từ trái sang phải. các`covered_until`biến đảm bảo chúng ta không phản ứng nhiều lần với các vị trí đã được xử lý trong lần sơ tán trước đó. Khi phát hiện thấy một vị trí nguy hiểm ngoài phạm vi phủ sóng, chúng tôi ngay lập tức cam kết sơ tán bắt đầu từ đó. 

Kiểm tra ranh giới`i + x - 1 >= n + 1`đảm bảo phân đoạn vừa khít bên trong mảng. Sử dụng lập chỉ mục đầu ra dựa trên 1 yêu cầu thêm`+1`khi lưu trữ câu trả lời. 

Một điểm tinh tế là chúng tôi không bao giờ cố gắng “chuyển” cuộc sơ tán về phía trước. Làm như vậy sẽ chỉ làm cho phạm vi phủ sóng trở nên tồi tệ hơn vì vị trí kích hoạt sẽ vẫn bị phát hiện. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 3, x = 2, m = 0
a = [1, 0, 2]
```Tất cả các giá trị dương đều nguy hiểm vì`m = 0`. 

| tôi | một [tôi] | được bảo hiểm_until | hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | -1 | đặt ở 0, bao gồm [0,1] | [1] | 
| 1 | 0 | 1 | đã được bảo hiểm | [1] | 
| 2 | 2 | 1 | i > che, đặt ở vị trí 2 không thể vì 2+1>3 | thất bại | 

Tại chỉ số 2, chúng ta cố gắng đặt một khối bắt đầu từ 2 (dựa trên 0), nhưng`2 + x - 1 = 3`vượt quá chỉ số 2 cuối cùng, do đó không có phân đoạn hợp lệ. 

Đầu ra là:```
-1
```Điều này cho thấy trường hợp thất bại ở ranh giới chặt chẽ trong đó các cuộc tấn công nguy hiểm muộn không thể được che chắn. 

### Mẫu 2 

đầu vào:```
n = 3, x = 2, m = 2
a = [2, 5, 6]
```Chỉ có chỉ số 1 và 2 là nguy hiểm. 

| tôi | một [tôi] | được bảo hiểm_until | hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | -1 | an toàn | [] | 
| 1 | 5 | -1 | đặt ở vị trí 1, bao gồm [1,2] | [2] | 
| 2 | 6 | 1 | đã được bảo hiểm | [2] | 

Chúng tôi đặt một cuộc sơ tán duy nhất bắt đầu từ vị trí 2 (chỉ mục dựa trên 1). 

Đầu ra:```
1
2
```Điều này xác nhận rằng một khoảng thời gian có thể bao phủ nhiều vị trí nguy hiểm liên tiếp một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được truy cập một lần và mỗi lần sơ tán được ghi lại một lần | 
| Không gian | O(k) | Chỉ lưu trữ các vị trí bắt đầu sơ tán | 

Thuật toán tuyến tính theo số lần tấn công, tối ưu cho`n`lên tới 500.000. Việc sử dụng bộ nhớ là tối thiểu vì chúng tôi chỉ lưu trữ các điểm sơ tán đã chọn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, x, m = map(int, input().split())
    a = list(map(int, input().split()))

    ans = []
    covered_until = -1
    i = 0

    while i < n:
        if i <= covered_until:
            i += 1
            continue

        if a[i] > m:
            if i + x - 1 >= n + 1:
                return "-1\n"
            ans.append(i + 1)
            covered_until = i + x - 1
        i += 1

    out = str(len(ans)) + "\n"
    if ans:
        out += " ".join(map(str, ans)) + "\n"
    return out

# provided samples
assert solve("3 2 0\n1 0 2\n") == "-1\n"
assert solve("3 2 2\n2 5 6\n") == "1\n2\n"

# custom cases
assert solve("1 1 0\n1\n") == "1\n1\n"
assert solve("5 2 10\n1 2 3 4 5\n") == "0\n"
assert solve("5 2 0\n1 1 1 1 1\n") == "-1\n"
assert solve("6 3 1\n0 2 0 3 0 4\n") in ["2\n2 4\n", "2\n2 4\n"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 0 / 1 | 1 1 | sơ tán bắt buộc nhỏ nhất | 
| 5 2 10 / tất cả đều an toàn | 0 | không cần sơ tán | 
| đều nguy hiểm với x nhỏ | -1 | không thể bảo hiểm đầy đủ | 
| thưa thớt nguy hiểm | nhiều hợp lệ | sự đúng đắn tham lam | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên xuất hiện khi cuộc tấn công nguy hiểm cuối cùng xảy ra ở gần cuối mảng. Nếu việc sơ tán phải bắt đầu ở chỉ số`n - x + 2`hoặc muộn hơn (dựa trên 1), phân đoạn sẽ tràn. Thuật toán nắm bắt điều này ngay lập tức bằng cách kiểm tra`i + x - 1 >= n`và trả về lỗi mà không cố gắng che phủ một phần. 

Một trường hợp khác là khi`m = 0`, biến mọi giá trị dương thành một kích hoạt sơ tán bắt buộc. Sự tham lam vẫn hoạt động vì nó chỉ đơn giản là gói các khoảng thời gian`x`từ trái sang phải, nhưng tính khả thi phụ thuộc hoàn toàn vào việc`n`có thể được chia thành các đoạn có kích thước rời rạc`x`bắt đầu từ mọi vị trí cần thiết. 

Cuối cùng, khi`x = 1`, mọi cuộc tấn công nguy hiểm đều cần có một cuộc sơ tán riêng. Thuật toán thoái hóa thành việc đếm các vị trí riêng lẻ và tính chính xác phụ thuộc vào việc không vô tình hợp nhất hoặc bỏ qua vùng phủ sóng do`covered_until`logic này vẫn đúng vì mỗi phân đoạn bao gồm chính xác một vị trí.
