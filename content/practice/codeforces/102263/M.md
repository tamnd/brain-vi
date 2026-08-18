---
title: "CF 102263M - Hai hoạt động"
description: "Chúng ta có một chuỗi các chữ cái viết thường. Chúng tôi có thể tự do hoán đổi hai vị trí bất kỳ, vì vậy thứ tự của các nhân vật không bao giờ là một hạn chế thực sự. Thao tác thứ hai lấy hai chữ cái liền kề bằng nhau và thay thế chúng bằng chữ cái tiếp theo."
date: "2026-08-17T20:11:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102263
codeforces_index: "M"
codeforces_contest_name: "ArabellaCPC 2019"
rating: 0
weight: 102263
solve_time_s: 72
verified: true
draft: false
---

[CF 102263M - Hai hoạt động](https://codeforces.com/problemset/problem/102263/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi các chữ cái viết thường. Chúng tôi có thể tự do hoán đổi hai vị trí bất kỳ, vì vậy thứ tự của các nhân vật không bao giờ là một hạn chế thực sự. Thao tác thứ hai lấy hai chữ cái liền kề bằng nhau và thay thế chúng bằng chữ cái tiếp theo. Ví dụ, hai`c`nhân vật có thể trở thành một`d`, trong khi hai`z`không thể gộp các ký tự vì không có ký tự nào sau`z`. 

Mục tiêu là thực hiện bất kỳ chuỗi thao tác nào và thu được chuỗi lớn nhất có thể về mặt từ điển. Vì có thể hoán đổi tùy ý nên khi biết mỗi chữ cái còn lại bao nhiêu bản sao, chúng ta luôn có thể sắp xếp chúng theo thứ tự giảm dần. Do đó, vấn đề thực sự là xác định tập hợp các chữ cái cuối cùng tốt nhất. 

Chiều dài có thể lớn bằng`10^5`. Nó đủ lớn để loại trừ các thuật toán liên tục khám phá nhiều chuỗi thao tác có thể hoặc quét toàn bộ chuỗi sau mỗi thao tác. Một giải pháp tuyến tính hoặc gần tuyến tính là phù hợp. Bảng chữ cái chỉ có 26 ký tự, cung cấp cho chúng ta một kích thước cố định đặc biệt hữu ích để truyền tải thông tin từ chữ cái này sang chữ cái tiếp theo. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể dẫn đến sai sót. Đối với đầu vào`aa`, đầu ra đúng là`b`. Chỉ cần sắp xếp chuỗi gốc sẽ tạo ra`aa`, thiếu thao tác cải thiện nghiêm ngặt ký tự đầu tiên. Vì`zz`, đầu ra đúng là`zz`, bởi vì`z`không có người kế vị. Việc triển khai tăng dần từng cặp một cách mù quáng sẽ tạo ra một ký tự ngoài bảng chữ cái không chính xác. Vì`yyy`, đầu ra đúng là`zy`: một cặp`y`trở thành`z`, trong khi phần còn lại`y`không thể tham gia vào một hoạt động khác. Cuối cùng, đối với`aaaa`, câu trả lời là`c`, không`bb`. Sau hai lần hợp nhất đầu tiên, chúng tôi có được`bb`, và hai cái đó`b`các ký tự có thể hợp nhất lại thành`c`. Giải pháp chỉ xử lý mỗi chữ cái một lần mà không đưa các cặp mới được tạo lên trên sẽ bỏ lỡ tầng này. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp có thể liên tục tìm kiếm hai chữ cái bằng nhau, hoán đổi các ký tự khi cần thiết để ghép một cặp như vậy lại với nhau, thay thế cặp đó bằng cặp kế tiếp và tiếp tục cho đến khi không còn cặp hữu ích nào. Điều này đúng nếu mọi cặp có sẵn cuối cùng đều được xử lý, bởi vì mọi thao tác đều giảm độ dài chuỗi đi một và các phép hoán đổi cho phép hai ký tự bằng nhau bất kỳ trở thành liền kề. 

Vấn đề là chi phí để thực hiện mô phỏng đó một cách hiệu quả. Có thể có nhiều như`n - 1`hoạt động. Mức tối đa này đạt được, chẳng hạn, khi chuỗi bao gồm số lũy thừa hai của`a`nhân vật. Với`n = 100000`, một cách triển khai đơn giản quét chuỗi hiện tại để tìm sự hợp nhất sau mỗi thao tác có thể thực hiện khoảng`100000 + 99999 + ... + 1 = 5,000,050,000`kiểm tra nhân vật trong trường hợp xấu nhất. Việc khám phá các thứ tự hoạt động khác nhau thậm chí còn tệ hơn vì nó có thể tạo ra một cây tìm kiếm lớn. 

Quan sát quan trọng là các giao dịch hoán đổi đã loại bỏ hoàn toàn tầm quan trọng của các vị thế. Chúng ta chỉ cần đếm từng chữ cái. Giả sử có`cnt[c]`bản sao của một số lá thư`c`. Mỗi cặp trong số chúng có thể được thay thế bằng một`c + 1`. Nếu có`cnt[c] // 2`cặp, tất cả chúng đều có thể được chuyển đổi, để lại`cnt[c] % 2`bản sao của`c`và thêm`cnt[c] // 2`sao chép sang chữ cái tiếp theo. 

Hoạt động này luôn có lợi cho việc tối đa hóa từ điển. Hai bản sao của`c`được thay thế bằng một bản sao của ký tự lớn hơn`c + 1`. Vì tất cả các ký tự còn lại có thể được sắp xếp theo thứ tự giảm dần nên việc di chuyển một ký tự lên trên trong bảng chữ cái sẽ mang lại sự đóng góp lớn nhất có thể cho các ký tự đó. Cũng không có lý do gì để bảo toàn một cặp ở chữ cái thấp hơn, bởi vì nó luôn có thể được chuyển đổi và bất kỳ cặp mới nào được tạo ở chữ cái tiếp theo đều có thể được chuyển đổi lại. 

Do đó toàn bộ quá trình trở thành một hoạt động vận chuyển nhỏ. Chúng tôi xử lý bảng chữ cái từ`a`bởi vì`y`. Tính chẵn lẻ của mỗi số đếm vẫn giữ nguyên ký tự hiện tại của nó, trong khi một nửa số đếm được chuyển sang ký tự tiếp theo. các`z`số lượng không bao giờ thay đổi, bởi vì`zz`không thể chuyển đổi được. Cuối cùng, chúng tôi xuất mọi ký tự còn lại từ`z`xuống`a`. 

Quá trình brute-force hoạt động vì mỗi lần hợp nhất đại diện cho một hoạt động hợp lệ, nhưng nó không thành công khi số lần hợp nhất trở thành tuyến tính và mỗi lần hợp nhất yêu cầu một lần quét khác. Quan sát cho thấy chỉ có số ký tự mới quan trọng cho phép chúng tôi thay thế tất cả các thao tác đó bằng 25 lần chuyển tiếp đếm theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem mỗi chữ cái trong số 26 chữ cái viết thường xuất hiện bao nhiêu lần trong chuỗi đầu vào. Vị trí không cần phải được giữ nguyên vì các hoán đổi tùy ý cho phép chúng ta sắp xếp lại chuỗi bất cứ khi nào cần thiết. 
2. Xử lý thư từ`a`ĐẾN`y`. Đối với chữ cái hiện tại có số lượng`cnt[i]`, giữ`cnt[i] % 2`sao chép lá thư này và thêm vào`cnt[i] // 2`sao chép vào`cnt[i + 1]`. Điều này chính xác thể hiện việc thực hiện tất cả các hợp nhất có thể có của ký tự hiện tại. 
3. Tiếp tục xử lý chữ cái tiếp theo bằng cách sử dụng số lượng đã cập nhật của nó. Bước này là bước xử lý các tầng như`aaaa -> bb -> c`. Các bản sao được tạo ở một cấp độ được xử lý giống hệt như các bản sao có trong đầu vào ban đầu. 
4. Để lại số lượng`z`không thay đổi. Không có ký tự nào sau`z`, vì vậy một cặp`z`ký tự không thể được chuyển đổi. 
5. Xây dựng câu trả lời bằng cách lặp lại từ`z`xuống`a`và nối thêm từng ký tự theo số đếm cuối cùng của nó. Thứ tự này là tối đa về mặt từ điển vì đối với một tập hợp cố định, việc đặt các ký tự lớn hơn trước đó luôn tạo ra chuỗi lớn nhất. 

Tại sao nó hoạt động: sau khi xử lý thư`i`, số cuối cùng của nó chính xác là số chẵn lẻ của số bản sao đạt tới nó, trong khi mọi cặp có thể có đều đã được chuyển đổi thành ký tự tiếp theo. Các cặp bị loại bỏ không bị mất vì mỗi cặp đóng góp chính xác một bản sao vào`i + 1`. Do đó, điều bất biến là tiền tố được xử lý của bảng chữ cái đã được giảm thiểu càng nhiều càng tốt, với mọi cặp còn lại được biểu thị ở vị trí tiếp theo. Vì không thể cải thiện thêm ký tự thấp hơn sau khi các cặp của nó được đưa lên trên và vì thứ tự giảm dần là tối ưu cho nhiều tập hợp kết quả nên chuỗi cuối cùng là tối đa về mặt từ điển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(s: str) -> str:
    cnt = [0] * 26

    for ch in s:
        cnt[ord(ch) - ord('a')] += 1

    for i in range(25):
        cnt[i + 1] += cnt[i] // 2
        cnt[i] %= 2

    ans = []

    for i in range(25, -1, -1):
        if cnt[i]:
            ans.append(chr(ord('a') + i) * cnt[i])

    return ''.join(ans)

def main():
    s = input().strip()
    print(solve(s))

if __name__ == "__main__":
    main()
```các`cnt`mảng lưu trữ bội số hiện tại của mỗi chữ cái. Việc đếm đầu vào cần một lần chuyển qua chuỗi và mỗi ký tự ánh xạ trực tiếp tới một chỉ mục bằng giá trị ASCII của nó. 

Vòng lặp từ chỉ mục`0`bởi vì`24`thực hiện thao tác mang từ mỗi chữ cái đến chữ cái kế tiếp. Thứ tự là cần thiết. Khi xử lý`b`, số đếm của nó có thể đã chứa các cặp được tạo từ`a`, do đó việc xử lý bảng chữ cái từ thấp đến cao sẽ tự động xử lý các chuỗi thao tác dài tùy ý. 

biểu hiện`cnt[i] // 2`là số cặp có thể được hợp nhất, trong khi`cnt[i] % 2`là số duy nhất phải giữ nguyên ở ký tự hiện tại. Số nguyên Python không có vấn đề tràn ở đây và mỗi số đếm tối đa bằng độ dài chuỗi ban đầu. 

Vòng lặp xây dựng chạy ngược lại để các chữ cái lớn hơn xuất hiện trước. Việc lặp lại từng ký tự theo số đếm cuối cùng của nó sẽ bảo toàn chính xác nhiều tập hợp cuối cùng được tạo ra bởi quá trình mang. 

Không cần phải mô phỏng rõ ràng các giao dịch hoán đổi. Mục đích duy nhất của chúng trong các thao tác ban đầu là làm cho các ký tự bằng nhau liền kề nhau và việc đếm các ký tự đã mang lại cho chúng ta sự tự do tương tự mà không cần tạo các chuỗi trung gian. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`abbx`, số đếm ban đầu là một`a`, hai`b`nhân vật và một`x`. 

| Thư đã được xử lý | Đếm trước | Cặp mang theo | Đếm được giữ | Đếm tiếp theo | 
| --- | --- | --- | --- | --- | 
|`a`| 1 | 0 | 1 |`b`còn lại 2 | 
|`b`| 2 | 1 | 0 |`c`trở thành 1 | 
|`c`| 1 | 0 | 1 |`d`vẫn là 0 | 
|`d`bởi vì`w`| 0 | 0 | 0 | không thay đổi | 
|`x`| 1 | 0 | 1 | không thay đổi | 
|`y`| 0 | 0 | 0 | không thay đổi | 
|`z`| 0 | không thể hợp nhất | 0 | không thay đổi | 

Multiset cuối cùng là`{x, c, a}`, do đó thứ tự giảm dần cho`xca`. Đây chính xác là kết quả mẫu. Dấu vết chứng tỏ rằng cặp`b`nhân vật trở thành`c`, và cái mới được tạo ra`c`được coi là một phần của cùng một quá trình thực hiện. 

Đối với mẫu thứ hai,`zyayz`, số đếm là một`a`, một`y`, một`z`, một`a`, và một`y`. 

| Thư đã được xử lý | Đếm trước | Cặp mang theo | Đếm được giữ | Hiệu ứng | 
| --- | --- | --- | --- | --- | 
|`a`| 2 | 1 | 0 | một`b`tạo | 
|`b`| 1 | 0 | 1 | còn lại`b`| 
|`c`bởi vì`x`| 0 | 0 | 0 | không thay đổi | 
|`y`| 2 | 1 | 0 | một`z`tạo | 
|`z`| 2 | không thể hợp nhất | 2 | còn lại`zz`| 

Số cuối cùng là hai`z`nhân vật và một`b`, cho`zzb`. Đầu ra mẫu là`zzza`, do đó dấu vết trực tiếp này cho thấy một sự điều chỉnh quan trọng: đầu vào trong câu lệnh được cung cấp thực tế là`zyayz`, số của nó là`z = 2`,`y = 2`,`a = 1`, không`a = 2`. Vì vậy, dấu vết thực tế là: 

| Thư đã được xử lý | Đếm trước | Cặp mang theo | Đếm được giữ | Hiệu ứng | 
| --- | --- | --- | --- | --- | 
|`a`| 1 | 0 | 1 |`a`còn lại | 
|`b`bởi vì`x`| 0 | 0 | 0 | không thay đổi | 
|`y`| 2 | 1 | 0 | một`z`tạo | 
|`z`| 3 | không thể hợp nhất | 3 | còn lại`zzz`| 

Chuỗi cuối cùng là`zzza`, phù hợp với mẫu Điều này thể hiện cách xử lý đặc biệt của`z`: cái`y`cặp có thể trở thành một cặp khác`z`, nhưng kết quả là ba`z`ký tự không thể được giảm thêm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc đếm quét chuỗi một lần và việc xử lý 26 chữ cái mất nhiều thời gian. | 
| Không gian | O(n) | Bản thân đầu ra có thể chứa các ký tự O(n); mảng đếm phụ sử dụng không gian O(1). | 

Với`n <= 10^5`, thuật toán thực hiện một lần chuyển qua đầu vào và một lần chuyển 26 chữ cái cố định sau đó. Nó tránh xây dựng mọi chuỗi trung gian và tránh quét lặp lại, do đó thời gian chạy của nó dễ dàng phù hợp với giới hạn đầu vào đã nêu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve(s: str) -> str:
    cnt = [0] * 26

    for ch in s:
        cnt[ord(ch) - ord('a')] += 1

    for i in range(25):
        cnt[i + 1] += cnt[i] // 2
        cnt[i] %= 2

    ans = []
    for i in range(25, -1, -1):
        ans.append(chr(ord('a') + i) * cnt[i])

    return ''.join(ans)

def run(inp: str) -> str:
    return solve(inp.strip())

# Provided samples
assert run("abbx") == "xca", "sample 1"
assert run("zyayz") == "zzza", "sample 2"

# Minimum-size input
assert run("a") == "a", "single character"

# z is a terminal character and can never be merged
assert run("zz") == "zz", "z boundary"

# Complete cascade: aaaa -> bb -> c
assert run("aaaa") == "c", "multi-level carry"

# y reaches z, but z cannot be carried further
assert run("yy") == "z", "y to z boundary"

# Odd count must leave one character behind
assert run("yyy") == "zy", "odd y count"

# A higher existing character must remain ahead of the carried result
assert run("aaz") == "zb", "existing z and generated b"

# Maximum-size input
assert run("z" * 100000) == "z" * 100000, "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`a`| Kích thước đầu vào tối thiểu và không thể thực hiện thao tác nào | 
|`zz`|`zz`| Ranh giới bảng chữ cái trên | 
|`aaaa`|`c`| Nhiều cấp độ mang liên tiếp | 
|`yy`|`z`| Chuyển đổi thành`z`mà không cần thử ký tự tiếp theo không tồn tại | 
|`yyy`|`zy`| Xử lý đúng số lẻ | 
|`aaz`|`zb`| Tương tác giữa các ký tự được tạo và ký tự cấp cao hiện có | 
|`z`lặp đi lặp lại 100000 lần | Cùng một chuỗi | Kích thước đầu vào tối đa và hiệu suất tuyến tính | 

## Vỏ cạnh 

Đối với đầu vào`aa`, thuật toán bắt đầu bằng`cnt[a] = 2`. Xử lý`a`mang một cặp đến`b`và để lại số không`a`ký tự, vì vậy số lượng cuối cùng của`b`là một. Đầu ra là`b`. Một giải pháp chỉ sắp xếp các ký tự gốc sẽ trả về không chính xác`aa`, bởi vì nó sẽ không bao giờ cho rằng việc hợp nhất hai ký tự thấp hơn sẽ tạo ra một ký tự lớn hơn về mặt từ điển. 

Đối với đầu vào`zz`, thuật toán không bao giờ xử lý`z`như một nguồn mang theo. Số đếm của nó vẫn bằng hai và cấu trúc giảm dần tạo ra`zz`. Ranh giới được xử lý bằng cách dừng vòng lặp tại`y`. Cố gắng truy cập phần kế thừa của`z`sẽ tạo ra một ký tự không hợp lệ hoặc gây ra lỗi lập chỉ mục. 

Vì`yyy`, số lượng`y`là ba. Một cặp được mang đến`z`, để lại một`y`. Số đếm cuối cùng là`z = 1`Và`y = 1`, vậy kết quả là`zy`. Thuật toán giữ bản sao lẻ thay vì loại bỏ nó, đây là điều kiện biên chính cho mỗi ký tự. 

Vì`aaaa`, lần mang đầu tiên thay đổi bốn`a`ký tự thành hai`b`nhân vật. Khi thuật toán đạt`b`, hai ký tự mới được tạo đó tạo thành một cặp khác và được mang tới`c`. Kết quả là một đơn`c`. Điều này chứng tỏ tại sao bảng chữ cái phải được xử lý từ trái sang phải: việc xử lý từng số đếm ban đầu một cách độc lập sẽ bỏ lỡ các thao tác được kích hoạt bởi các ký tự được tạo trước đó. 

Vì`aaz`, hai`a`nhân vật trở thành một`b`, trong khi bản gốc`z`vẫn còn nguyên. Multiset cuối cùng là`{z, b}`, và thứ tự giảm dần tạo ra`zb`. Điều này xác nhận rằng thao tác không cần phải được thực hiện trước trên ký tự hiện có cao nhất. Tất cả các mức mang cấp thấp hơn có thể được tính toán độc lập và sau đó được hợp nhất thành thứ tự cuối cùng. 

Đối với đầu vào chỉ chứa`z`, chẳng hạn như`z`, không thể thực hiện được thao tác nào cả. Mảng đếm không thay đổi và kết quả đầu ra chính xác`z`, điều này cũng xác nhận rằng thuật toán xử lý đầu vào nhỏ nhất có thể mà không dựa vào bất kỳ cặp nào hiện có.
