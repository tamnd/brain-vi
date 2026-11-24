---
title: "CF 959A - Mahmoud và Ehab và trò chơi chẵn"
description: "Trò chơi bắt đầu với một giá trị số nguyên duy nhất và hai người chơi luân phiên giảm giá trị đó. Đến lượt Mahmoud, anh ta chỉ được phép trừ một số dương chẵn không vượt quá giá trị hiện tại."
date: "2026-06-17T01:53:56+07:00"
tags: ["codeforces", "competitive-programming", "games", "math"]
categories: ["algorithms"]
codeforces_contest: 959
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 473 (Div. 2)"
rating: 800
weight: 959
solve_time_s: 58
verified: true
draft: false
---

[CF 959A - Mahmoud và Ehab và trò chơi chẵn](https://codeforces.com/problemset/problem/959/A) 

**Đánh giá:** 800 
**Tags:** trò chơi, toán học 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trò chơi bắt đầu với một giá trị số nguyên duy nhất và hai người chơi luân phiên giảm giá trị đó. Đến lượt Mahmoud, anh ta chỉ được phép trừ một số dương chẵn không vượt quá giá trị hiện tại. Đến lượt Ehab, anh ta chỉ được phép trừ một số dương lẻ trong cùng một giới hạn. Nếu một người chơi không có phép trừ hợp lệ trong lượt của mình, người chơi đó sẽ thua ngay lập tức. 

Khía cạnh quan trọng là trò chơi được xác định hoàn toàn bởi cấu trúc chẵn lẻ của các nước đi được phép chứ không phải bởi bất kỳ lựa chọn chiến lược nào giữa nhiều giá trị. Mỗi nước đi đều giảm số lượng một cách nghiêm ngặt, do đó trò chơi phải kết thúc sau hữu hạn nhiều bước và cách chơi tối ưu giảm xuống việc xác định xem vị trí xuất phát là thắng hay thua đối với người chơi đầu tiên. 

Ràng buộc kích thước đầu vào cho phép n lên tới 10^9. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào cố gắng khám phá các trạng thái của cây trò chơi, vì ngay cả việc truyền tuyến tính trên tất cả các trạng thái cho đến n cũng sẽ quá chậm. Bất kỳ giải pháp hợp lệ nào cũng phải nén trò chơi thành một quyết định liên tục theo thời gian dựa trên thuộc tính của n. 

Trường hợp cạnh tinh tế xảy ra khi n rất nhỏ. Ví dụ: khi n = 1, Mahmoud không thể trừ bất kỳ số chẵn nào nên anh ta thua ngay lập tức. Khi n = 2, Mahmoud có thể trừ 2, để lại 0 cho Ehab, người không còn nước đi lẻ nào nữa, vì vậy Mahmoud thắng. Những trường hợp nhỏ này tiết lộ rằng tính chẵn lẻ của các nước đi sẵn có quan trọng hơn độ lớn của chính n. 

Một cạm bẫy tiềm ẩn khác là giả định rằng cả hai người chơi luôn có thể di chuyển cho đến khi n trở thành 0. Điều này sai vì tính chẵn lẻ được phép hạn chế các bước đi có sẵn một cách không đối xứng, do đó một người chơi có thể bị buộc vào thế thua sớm hơn dự kiến. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng trò chơi. Ở mỗi bước, chúng tôi thử tất cả các nước đi hợp lệ cho người chơi hiện tại, đánh giá đệ quy các trạng thái kết quả và xác định xem có nước đi nào dẫn đến vị trí thua cho đối thủ hay không. Đây là một trò chơi minimax tiêu chuẩn trên một dòng trạng thái từ 0 đến n. 

Điều này hoạt động về mặt khái niệm vì trò chơi là hữu hạn và mang tính quyết định. Tuy nhiên, từ bất kỳ trạng thái n nào, Mahmoud có thể thử tới n/2 nước đi chẵn và Ehab có thể thử tới (n+1)/2 nước đi lẻ. Mỗi bước di chuyển làm giảm trạng thái nhưng tạo ra một nhánh mới. Trong trường hợp xấu nhất, đệ quy khám phá một phần lớn các trạng thái một cách lặp đi lặp lại, dẫn đến hành vi theo cấp số nhân nếu không sử dụng tính năng ghi nhớ và ít nhất là hành vi tuyến tính ngay cả với tính năng ghi nhớ. Vì n có thể là 10^9 nên ngay cả O(n) cũng không thể. 

Nhận xét quan trọng là cấu trúc của các nước đi được phép sẽ đưa trò chơi về trạng thái chẵn lẻ đơn giản. Mahmoud chỉ có thể di chuyển nếu tồn tại ít nhất một số chẵn ≤ n, điều này xảy ra chính xác khi n ≥ 2. Ehab luôn có thể di chuyển bất cứ khi nào n ≥ 1, vì 1 luôn là một phép trừ lẻ hợp lệ. Sự tương tác giữa các ràng buộc này cho thấy sự khác biệt có ý nghĩa duy nhất là n chẵn hay lẻ khi bắt đầu. Nếu n chẵn, Mahmoud có thể thực hiện một nước đi để bảo toàn cấu trúc theo cách buộc Ehab vào thế không thể tiếp tục giành chiến thắng. Nếu n là số lẻ, Mahmoud ngay lập tức không thể thực hiện một nước đi đồng đều để ổn định hoàn toàn lợi thế và Ehab sẽ nắm quyền kiểm soát. 

Điều này làm cho toàn bộ trò chơi chỉ còn việc kiểm tra tính chẵn lẻ của n. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) hoặc tệ hơn | O(n) | Quá chậm | 
| Quan sát chẵn lẻ | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc số nguyên n, biểu thị trạng thái ban đầu của trò chơi. 
2. Kiểm tra xem n là chẵn hay lẻ, vì chỉ có tính chẵn lẻ mới xác định liệu Mahmoud có nước đi đầu tiên thắng hay không. 
3. Nếu n lẻ, kết luận Mahmoud không thể thực hiện nước đi để lại thế có lợi nên Ehab thắng ngay. 
4. Nếu n chẵn, ban đầu Mahmoud có thể trừ 2, để lại một số lẻ cho Ehab, sau đó Mahmoud luôn có thể đáp trả các nước đi lẻ bắt buộc của Ehab theo cách duy trì quyền kiểm soát lợi thế chẵn lẻ, vì vậy Mahmoud thắng. 

### Tại sao nó hoạt động 

Trò chơi giảm bớt việc theo dõi người chơi nào bị buộc phải đối mặt với một vị trí mà trước tiên không có nước đi hợp pháp nào tồn tại. Mahmoud bị hạn chế trong các bước di chuyển chẵn, điều này có nghĩa là anh ta mất kiểm soát khi trạng thái bắt đầu là lẻ vì anh ta không thể phản ứng ngay lập tức theo cách duy trì mô hình giảm cân bằng. Khi n chẵn, Mahmoud luôn có thể thực hiện phép quy giản đầu tiên về trạng thái lẻ, sau đó Ehab bị buộc vào thế chẵn lẻ bất lợi. Vì mọi nước đi đều làm giảm nghiêm ngặt n và các lần thay thế chẵn lẻ trong cách chơi tối ưu, nên số chẵn lẻ ban đầu sẽ xác định đầy đủ người chơi nào cuối cùng sẽ bị chặn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

if n % 2 == 1:
    print("Ehab")
else:
    print("Mahmoud")
```Giải pháp đọc một số nguyên duy nhất và ngay lập tức kiểm tra tính chẵn lẻ của nó. Không cần theo dõi hoặc mô phỏng trạng thái vì trò chơi chuyển sang phân loại nhị phân. Hoạt động modulo là thời gian không đổi và việc in kết quả sẽ hoàn thiện quyết định. 

Chi tiết triển khai tinh tế duy nhất là đảm bảo xử lý tính chẵn lẻ chính xác cho n = 1 và n = 2. Những điều này đã được bao hàm bởi cùng một điều kiện, vì 1 là số lẻ và 2 là số chẵn, phù hợp với các kết quả đã biết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào: n = 1 

| Xoay | Người chơi | n trước | Di chuyển | n sau | 
| --- | --- | --- | --- | --- | 
| 1 | Mahmoud | 1 | thậm chí không có giá trị di chuyển | thiết bị đầu cuối | 

Mahmoud không có động thái hợp pháp khi bắt đầu nên trận đấu kết thúc ngay lập tức với việc Mahmoud thua cuộc. Điều này khẳng định quy luật n lẻ dẫn đến chiến thắng của Ehab. 

### Ví dụ 2 

Đầu vào: n = 2 

| Xoay | Người chơi | n trước | Di chuyển | n sau | 
| --- | --- | --- | --- | --- | 
| 1 | Mahmoud | 2 | trừ 2 | 0 | 
| 2 | Ehab | 0 | không có nước đi lẻ hợp lệ | thiết bị đầu cuối | 

Mahmoud buộc trò chơi rơi vào trạng thái cuối mà Ehab không thể phản hồi. Điều này khẳng định rằng ngay cả n cũng dẫn đến chiến thắng của Mahmoud. 

Hai dấu vết này chứng tỏ rằng toàn bộ kết quả của trò chơi được cố định ngay lập tức bởi tính chẵn lẻ của trạng thái ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ kiểm tra và xuất ra tính chẵn lẻ | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Ràng buộc đầu vào cho phép tối đa 10^9, do đó, mọi cách tiếp cận tuyến tính hoặc đệ quy đều không khả thi. Kiểm tra tính chẵn lẻ theo thời gian không đổi phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    return "Ehab" if n % 2 == 1 else "Mahmoud"

# provided samples
assert run("1\n") == "Ehab", "sample 1"
assert run("2\n") == "Mahmoud", "sample 2"

# custom cases
assert run("3\n") == "Ehab", "small odd"
assert run("4\n") == "Mahmoud", "small even"
assert run("1000000000\n") == "Mahmoud", "large even"
assert run("999999999\n") == "Ehab", "large odd"
assert run("5\n") == "Ehab", "odd mid case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | Ehab | trường hợp thua tối thiểu cho Mahmoud | 
| 2 | Mahmoud | trường hợp thắng nhỏ nhất | 
| 1000000000 | Mahmoud | ranh giới lớn thậm chí đầu vào | 
| 999999999 | Ehab | đầu vào lẻ ​​ranh giới lớn | 
| 5 | Ehab | tính nhất quán của trường hợp lẻ điển hình | 

## Vỏ cạnh 

Với n = 1, thuật toán kiểm tra tính chẵn lẻ và đưa ra Ehab. Điều này phù hợp với thực tế là Mahmoud thậm chí không hề di chuyển chút nào nên trò chơi kết thúc ngay ở lượt đầu tiên. 

Với n = 2, thuật toán đưa ra Mahmoud. Theo logic, n chẵn nên Mahmoud được tuyên bố là người chiến thắng, phù hợp với trình tự nước đi mà anh ta trừ 2 và buộc Ehab vào n = 0 mà không có phép trừ lẻ hợp pháp. 

Đối với n rất lớn chẳng hạn như 10^9, việc tính toán vẫn giống hệt nhau vì chỉ ước tính modulo 2 và không có phép lặp nào phụ thuộc vào độ lớn.
